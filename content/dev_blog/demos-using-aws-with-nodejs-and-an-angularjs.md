{
    "slug": "demos-using-aws-with-nodejs-and-an-angularjs",
    "date": "2014-02-01T19:45:00.000Z",
    "tags": [
        "angularjs",
        "nodejs",
        "aws",
        "amazonaws",
        "node.js",
        "socket.io",
        "dynamodb",
        "rds",
        "s3",
        "ses",
        "sns",
        "elasticbeanstalk",
        "ebs"
    ],
    "title": "Demos using AWS with Node.JS and an AngularJS frontend",
    "publishdate": "2014-02-01T19:45:00.000Z"
}Demos using AWS with Node.JS and an AngularJS frontend
======================================================




<p>I recently decided to build some reusable code for a bunch of projects that I&rsquo;ve got queued up. I wanted some backend components that leveraged a few of the highly scalable Amazon AWS services. This ended up taking me a month to finish, which is way longer than the week that I had intended to spend on it. (It was a month of nights and weekends, not as much free time as I&rsquo;d hoped for in January.) <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/" target="_blank">Oh, and before I forget, here&rsquo;s the GitHub repo</a>.</p>

<p>This project&rsquo;s goal is to build small demo utilities that should be a
reasonable approximation of what we might see in an application that uses the aws-sdk node.js module. AngularJS will serve as a front-end, with no direct access to the AWS libraries, and will use the node server to handle all requests.</p>

<p><a href="http://awsnodeangulardemos-env.elasticbeanstalk.com/" target="_blank">Here&rsquo;s a temporary EBS instance running to demonstrate this</a>.
It will be taken down in a few weeks, so don&rsquo;t get so attached to it. I might migrate it to my other server, so hopefully while the URL might change, the service will remain alive.</p>

<h2>ToC</h2>

<ol><li><a href="#dataset" target="_blank">Data Set</a></li>
<li><a href="#dynamo" target="_blank">DynamoDB</a></li>
<li><a href="#rds" target="_blank">RDS</a></li>
<li><a href="#s3" target="_blank">S3</a></li>
<li><a href="#ses" target="_blank">SES</a></li>
<li><a href="#sns" target="_blank">SNS</a></li>
<li><a href="#angularjs" target="_blank">AngularJS</a></li>
<li><a href="#ebs_deploy" target="_blank">Elastic Beanstalk Deployment</a></li>
</ol><h2><a name="dataset">Data Set</a></h2>

<p>Both DynamoDB and RDS are going to use the same basic data set. It will be a very simple set, with two tables that are JOINable. Here&rsquo;s the schema:</p>

<pre><code>Users: {
    id: 0,
    name: "Steve",
    email: "steve@example.com"
}

Media: {
    id: 0,
    uid: 0,
    url: "http://example.com/image.jpg",
    type: "image/jpg"
}
</code></pre>

<p>The same schema will be used for both Dynamo and RDS, almost. RDS uses an mkey field in the media table, to keep track of the key. Dynamo uses a string id, which should be the key of the media object in S3.</p>

<h2><a name="dynamo">DynamoDB</a></h2>

<p>Using the above schema, we set up a couple Dynamo tables. These can be treated in a similar way to how you would treat any NoSQL database, except that Dynamo&rsquo;s API is a bit onerous. I&rsquo;m not sure why they insisted on not using standard JSON, but a converter can be easily written to go back and forth between Dynamo&rsquo;s JSON format, and the normal JSON that you&rsquo;ll want to work with. <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/utils/dynamo_to_json.js" target="_blank">Take a look at how the converter works</a>. Also, <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/dynamo-demo/users.js" target="_blank">check out some other dynamo code here</a>.</p>

<p>There are just a couple of things going on in the DynamoDB demo. We have a method for getting all the users, adding or updating a user (if the user has the same id), and deleting a user. The <code>getAll</code> method does a <code>scan</code> on the Dynamo table, but only returns 100 results. It&rsquo;s a good idea to limit your results, and then load more as the user requests.</p>

<p>The <code>addUpdateUser</code> method takes in a user object, generates an id based off of the hash of the email, then does a <code>putItem</code> to Dynamo, which will either create a new entry, or update a current one. Finally, <code>deleteUser</code> runs the Dynamo API method <code>deleteItem</code>.</p>

<p>The following are a few methods that you&rsquo;ll find in the node.js code. Essentially, the basics are there, and we spit the results out over a socket.io socket. The socket will be used throughout most of the examples.</p>

<h3>Initialize</h3>

<pre class="pre-scrollable">
AWS.config.region = "us-east-1";
AWS.config.apiVersions = {
    dynamodb: '2012-08-10',
};
var dynamodb = new AWS.DynamoDB();
</pre>

<h3>Get all the users</h3>

<pre class="pre-scrollable">
var getAll = function (socket) {
    dynamodb.scan({
        "TableName": c.DYN_USERS_TABLE,
        "Limit": 100
    }, function (err, data) {
    if (err) {
        socket.emit(c.DYN_GET_USERS, "error");
    } else {
        var finalData = converter.ArrayConverter(data.Items);
        socket.emit(c.DYN_GET_USERS, finalData);
    }
    });
};
</pre>

<h3>Insert or update a user</h3>

<pre class="pre-scrollable">
var addUpdateUser = function (user, socket) {
    user.id = genIdFromEmail(user.email);
    var userObj = converter.ConvertFromJson(user);
    dynamodb.putItem({
        "TableName": c.DYN_USERS_TABLE,
        "Item": userObj
    }, function (err, data) {
        if (err) {
            socket.emit(c.DYN_UPDATE_USER, "error");
        } else {
           socket.emit(c.DYN_UPDATE_USER, data);
        }
    });
};
</pre>

<h3>Delete a user</h3>

<pre class="pre-scrollable">
var deleteUser = function (userId, socket) {
    var userObj = converter.ConvertFromJson({id: userId});
    dynamodb.deleteItem({
        "TableName": c.DYN_USERS_TABLE,
        "Key": userObj
    }, function (err, data) {
        if (err) {
            socket.emit(c.DYN_DELETE_USER, "error");
        } else {
            socket.emit(c.DYN_DELETE_USER, data);
        }
    });
};
</pre>

<h2><a name="rds">RDS</a></h2>

<p>This one&rsquo;s pretty simple, RDS gives you an olde fashioned SQL database server. It&rsquo;s so common that I had to add the &lsquo;e&rsquo; to the end of old, to make sure you understand just how common this is. Pick your favorite database server, fire it up, then use whichever node module works best for you. There&rsquo;s a bit of setup and configuration, which I&rsquo;ll dive into in the blog post. <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/rds-demo/users.js" target="_blank">Here&rsquo;s the code.</a></p>

<p>I&rsquo;m not sure that there&rsquo;s even much to talk about with this one. This example uses the <code>mysql</code> npm module, and is really, really straightforward. We need to start off by connecting to our DB, but that&rsquo;s about it. The only thing you&rsquo;ll need to figure out is the deployment of RDS, and making sure that you&rsquo;re able to connect to it, but that&rsquo;s a very standard topic, that I&rsquo;m not going to cover here since there&rsquo;s nothing specific to node.js or AngularJS.</p>

<p>The following are a few methods that you&rsquo;ll find in the node.js code. Essentially, the basics are there, and we spit the results out over a socket.io socket. The socket will be used throughout most of the examples.</p>

<h3>Initialize</h3>

<pre class="pre-scrollable">
AWS.config.region = "us-east-1";
AWS.config.apiVersions = {
    rds: '2013-09-09',
};
var rds_conf = {
    host: mysqlHost,
    database: "aws_node_demo",
    user: mysqlUserName,
    password: mysqlPassword
};
var mysql = require('mysql');
var connection = mysql.createConnection(rds_conf);
var rds = new AWS.RDS();
connection.connect(function(err){
    if (err)
        console.error("couldn't connect",err);
    else
        console.log("mysql connected");
});
</pre>

<h3>Get all the users</h3>

<pre class="pre-scrollable">
var getAll = function(socket){
    var query = this.connection.query('select * from users;',
      function(err,result){
        if (err){
            socket.emit(c.RDS_GET_USERS, c.ERROR);
        } else {
            socket.emit(c.RDS_GET_USERS, result);
        }
    });
};
</pre>

<h3>Insert or update a user</h3>

<pre class="pre-scrollable">
var addUpdateUser = function(user, socket){
    var query = this.connection.query('INSERT INTO users SET ?',
      user, function(err, result) {
        if (err) {
            socket.emit(c.RDS_UPDATE_USER, c.ERROR);
        } else {
            socket.emit(c.RDS_UPDATE_USER, result);
        }
    });
};
</pre>

<h3>Delete a user</h3>

<pre class="pre-scrollable">
var deleteUser = function(userId, socket){
    var query = self.connection.query('DELETE FROM users WHERE id = ?',
      userId, function(err, result) {
        if (err) {
            socket.emit(c.RDS_DELETE_USER, c.ERROR);
        } else {
            socket.emit(c.RDS_DELETE_USER, result);
        }
    });
};
</pre>

<h2><a name="s3">S3</a></h2>

<p>This one was a little tricky, but basically, we&rsquo;re just generating a unique random key and using that to keep track of the object. We then generate both GET and PUT URLs on the node.js server, so that the client does not have access to our AWS auth tokens. The client only gets passed the URLs it needs. <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/s3-demo/s3_utils.js" target="_blank">Check out the code!</a></p>

<p>The <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/s3-demo/s3_utils.js" target="_blank">s3_utils.js</a> file is very simple. <code>listBuckets</code> is a method to
verify that you&rsquo;re up and running, and lists out your current s3 buckets. Next up, <code>generateUrlPair</code> is simple, but important. Essentially, what we want is a way for the client to push things up to S3, without having our credentials. To accomplish this, we can generate signed URLs on the server, and pass those back to the client, for the client to use. This was a bit tricky to do, because there are a lot of important details, like making
certain that the client uses the same exact content type when it attempts to PUT the object. We&rsquo;re also making it world readable, so instead of creating a signed GET URL, we&rsquo;re just calculating the publicly accessible GET URL and returning that. The key for the object is random, so we don&rsquo;t need to know anything about the object we&rsquo;re uploading ahead of time.
(However, this demo assumes that only images will be uploaded, for simplicity.) Finally, <code>deleteMedia</code> is simple, we just use the S3 API to delete the object.</p>

<p>There are actually two versions of the S3 demo, the DynamoDB version, and the S3 version. For Dynamo, we use the <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/dynamo-demo/media.js" target="_blank">Dynamo media.js</a> file. Similarly, for the RDS version, we use the <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/rds-demo/media.js" target="_blank">RDS media.js</a>.</p>

<p>Looking first at the Dynamo version, <code>getAll</code> is not very useful, since we don&rsquo;t really want to see everyone&rsquo;s media, I don&rsquo;t think this even gets called. The methods here are very similar to those in user.js, we leverage the <code>scan</code>, <code>putItem</code>, and <code>deleteItem</code> APIs.</p>

<p>The same is true of the RDS version with respect to our previous RDS example. We&rsquo;re just making standard SQL calls, just like we did before.</p>

<p>You&rsquo;ll need to modify the CORS settings on your S3 bucket for this to work. Try the following configuration:</p>

<pre>
    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"&gt;
        &lt;CORSRule&gt;
            &lt;AllowedOrigin&gt;*&lt;/AllowedOrigin&gt;
            &lt;AllowedOrigin&gt;http://localhost:3000&lt;/AllowedOrigin&gt;
            &lt;AllowedMethod&gt;GET&lt;/AllowedMethod&gt;
            &lt;AllowedMethod&gt;PUT&lt;/AllowedMethod&gt;
            &lt;AllowedMethod&gt;POST&lt;/AllowedMethod&gt;
            &lt;AllowedMethod&gt;DELETE&lt;/AllowedMethod&gt;
            &lt;MaxAgeSeconds&gt;3000&lt;/MaxAgeSeconds&gt;
            &lt;AllowedHeader&gt;Content-*&lt;/AllowedHeader&gt;
            &lt;AllowedHeader&gt;Authorization&lt;/AllowedHeader&gt;
            &lt;AllowedHeader&gt;*&lt;/AllowedHeader&gt;
        &lt;/CORSRule&gt;
    &lt;/CORSConfiguration&gt;
</pre>

<p>The following are a few methods that you&rsquo;ll find in the node.js code. Essentially, the basics are there, and we spit the results out over a socket.io socket. The socket will be used throughout most of the examples.</p>

<h3>Initialize</h3>

<pre class="pre-scrollable">
AWS.config.region = "us-east-1";
AWS.config.apiVersions = {
    s3: '2006-03-01',
};
var s3 = new AWS.S3();

</pre>

<h3>Generate signed URL pair</h3>

<p>The GET URL is public, since that&rsquo;s how we want it. We could have easily
generated a signed GET URL, and kept the objects in the bucket private.</p>

<pre class="pre-scrollable">
var generateUrlPair = function (socket) {
    var urlPair = {};
    var key = genRandomKeyString();
    urlPair[c.S3_KEY] = key;
    var putParams = {Bucket: c.S3_BUCKET, Key: key, ACL: "public-read", ContentType: "application/octet-stream" };
    s3.getSignedUrl('putObject', putParams, function (err, url) {
        if (!!err) {
            socket.emit(c.S3_GET_URLPAIR, c.ERROR);
            return;
        }
        urlPair[c.S3_PUT_URL] = url;
        urlPair[c.S3_GET_URL] = "https://aws-node-demos.s3.amazonaws.com/" + qs.escape(key);
        socket.emit(c.S3_GET_URLPAIR, urlPair);
    });
};
</pre>

<h3>Delete Object from bucket</h3>

<pre class="pre-scrollable">
var deleteMedia = function(key, socket) {
    var params = {Bucket: c.S3_BUCKET, Key: key};
    s3.deleteObject(params, function(err, data){
        if (!!err) {
            socket.emit(c.S3_DELETE, c.ERROR);
            return;
        }
        socket.emit(c.S3_DELETE, data);
    });
}
</pre>

<h3>Client-side send file</h3>

<pre><code>var sendFile = function(file, url, getUrl) {
    var xhr = new XMLHttpRequest();
    xhr.file = file; // not necessary if you create scopes like this
    xhr.addEventListener('progress', function(e) {
        var done = e.position || e.loaded, total = e.totalSize || e.total;
        var prcnt = Math.floor(done/total*1000)/10;
        if (prcnt % 5 === 0)
            console.log('xhr progress: ' + (Math.floor(done/total*1000)/10) + '%');
    }, false);
    if ( xhr.upload ) {
        xhr.upload.onprogress = function(e) {
            var done = e.position || e.loaded, total = e.totalSize || e.total;
            var prcnt = Math.floor(done/total*1000)/10;
            if (prcnt % 5 === 0)
                console.log('xhr.upload progress: ' + done + ' / ' + total + ' = ' + (Math.floor(done/total*1000)/10) + '%');
        };
    }
    xhr.onreadystatechange = function(e) {
        if ( 4 == this.readyState ) {
            console.log(['xhr upload complete', e]);
            // emit the 'file uploaded' event
            $rootScope.$broadcast(Constants.S3_FILE_DONE, getUrl);
        }
    };
    xhr.open('PUT', url, true);
    xhr.setRequestHeader("Content-Type","application/octet-stream");
    xhr.send(file);
}
</code></pre>

<h2><a name="ses">SES</a></h2>

<p>SES uses another DynamoDB table to track emails that have been sent. We want to ensure that users have the ability to unsubscribe, and we don&rsquo;t want people sending them multiple messages. Here&rsquo;s the schema for the Dynamo table:</p>

<pre><code> Emails: {
     email: "steve@example.com",
     count: 1
 }
</code></pre>

<p>That&rsquo;s it! We&rsquo;re just going to check if the email is in that table, and what the count is before doing anything, then update the record after the email has been sent. <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/ses-demo/user_activity.js" target="_blank">Take a look at how it works.</a></p>

<p>Sending email with SES is fairly simple, however getting it to production requires jumping through a couple extra hoops. Basically, you&rsquo;ll need to use <a href="#sns" target="_blank">SNS</a> to keep track of bounces and complaints.</p>

<p>What we&rsquo;re doing here is for a given user, grab all their media, package it up in some auto-generated HTML, then use the <code>sendEmail</code> API call to actually send the message. We are also keeping track of the number of times we send each user an email. Since this is just a stupid demo that I&rsquo;m hoping can live on auto-pilot for a bit, I set a very low limit on the number of emails that may be sent to a particular address. Emails also
come with a helpful 'ubsubscribe&rsquo; link.</p>

<p>The following are a few methods that you&rsquo;ll find in the node.js code. Essentially, the basics are there, and we spit the results out over a socket.io socket. The socket will be used throughout most of the examples.</p>

<h3>Initialize</h3>

<pre class="pre-scrollable">
AWS.config.region = "us-east-1";
AWS.config.apiVersions = {
    sns: '2010-03-31',
    ses: '2010-12-01'
};
var ses = new AWS.SES();
</pre>

<h3>Send Email</h3>

<pre class="pre-scrollable">
var sendEmail = function (user, userMedia, socket) {
    var params = {
        Source: "me@mydomain.com",
        Destination: {
            ToAddresses: [user.email]
        },
        Message: {
            Subject: {
                Data: user.name + "'s media"
            },
            Body: {
                Text: {
                    Data: "please enable HTML to view this message"
                },
                Html: {
                    Data: getHtmlBodyFor(user, userMedia)
                }
            }
        }
    };
    ses.sendEmail(params, function (err, data) {
        if (err) {
            socket.emit(c.SES_SEND_EMAIL, c.ERROR);
        } else {
            socket.emit(c.SES_SEND_EMAIL, c.SUCCESS);
        }
    });
};
</pre>

<h2><a name="sns">SNS</a></h2>

<p>We&rsquo;re also listening for SNS messages to tell us if there&rsquo;s an email that&rsquo;s bounced or has a complaint. In the case that we get something, we immediately add an entry to the <code>Emails</code> table with a count of 1000. We will never attempt to send to that email address again.</p>

<p>I have my SES configured to tell SNS to send REST requests to my service, so that I can simply parse out the HTML, and grab the data that I need that way. Some of this is done in <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/app.js" target="_blank">app.js</a>, and the rest is handled in <a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/blob/master/ses-demo/bounces.js" target="_blank">bounces.js</a>. In bounces, we first need to verify with SNS that we&rsquo;re receiving the requests and handling them properly. That&rsquo;s what <code>confirmSubscription</code> is all about. Then, in <code>handleBounce</code> we deal with any complaints and bounces by unsubscribing the email.</p>

<h2><a name="angularjs">AngularJS</a></h2>

<p>The AngularJS code for this is pretty straightforward. Essentially, we just have a service for our socket.io connection, and to keep track of data coming in from Dynamo and RDS. There are controllers for each of the different views that we have, and they also coordinate with the services. We are also leveraging Angular&rsquo;s built-in events system, to inform various pieces about when things get updated.</p>

<p>There&rsquo;s nothing special about the AngularJS code here, we use socket.io to shuffle data to and from the server, then dump it to the UI with the normal bindings. I do use Angular events which I will discuss in a separate post.</p>

<h2><a name="ebs_deploy">Elastic Beanstalk Deployment</a></h2>

<p><a href="http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_nodejs.sdlc.html" target="_blank">Here&rsquo;s the AWS doc on setting up deployment with git integration straight from your project</a>. It&rsquo;s super simple. What&rsquo;s not so straightforward, however, is that you need to make sure that the ports are set up correctly. If you can just run your node server on port 80, that&rsquo;s the easiest thing, but I don&rsquo;t think that the instance that you get from Amazon will allow you to do that. So, you&rsquo;ll need to configure your LoadBalancer to forward port 80 to whatever port you&rsquo;re running on, then open that port in the EC2 Security Group that the Beanstalk environment is running in.</p>

<p>Once again, do use the git command-line deployment tools, as it allows you to deploy in one line after a <code>git commit</code>, using <code>git aws.push</code>.</p>

<p>A couple of other notes about the deployment. First, you&rsquo;re going to need to make sure that the node.js version is set correctly, AWS Elastic Beanstalk currently supports up to v0.10.21, but runs an earlier version by default. You will also need to add several environment variables from the console. I use the following parameters:</p>

<ul><li>AWS_ACCESS_KEY_ID</li>
<li>AWS_SECRET_ACCESS_KEY</li>
<li>AWS_RDS_HOST</li>
<li>AWS_RDS_MYSQL_USERNAME</li>
<li>AWS_RDS_MYSQL_PASSWORD</li>
</ul><p>Doing this allowed me to not ever commit sensitive data. To get there, log into your AWS console, then go to Elastic Beanstalk and select your environment. Navigate to 'Configuration&rsquo;, then to 'Software Configuration&rsquo;. From here you can set the node.js version, and add environment variables. You&rsquo;ll need to add the custom names above along with the values. If you&rsquo;re deploying to your own box, you&rsquo;ll need to at least export the above environment variables:</p>

<pre>
export AWS_ACCESS_KEY_ID='AKID'
export AWS_SECRET_ACCESS_KEY='SECRET'
export AWS_RDS_HOST='hostname'
export AWS_RDS_MYSQL_USERNAME='username'
export AWS_RDS_MYSQL_PASSWORD='pass'
</pre>

<p><a href="https://github.com/emil10001/AWS-NodeJS-AngularJS-Demos/" target="_blank">Again, the GitHub repo</a>.</p>
