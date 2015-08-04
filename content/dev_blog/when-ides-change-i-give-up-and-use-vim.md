{
    "slug": "when-ides-change-i-give-up-and-use-vim",
    "date": "2013-05-20T01:10:00.000Z",
    "tags": [
        "android",
        "eclipse",
        "intellij",
        "vim"
    ],
    "title": "When IDEs change, I give up and use vim",
    "publishdate": "2013-05-20T01:10:00.000Z"
}When IDEs change, I give up and use vim
=======================================




<p>As you may have read, Android is switching its main IDE from Eclipse (ADT Bundle) to Intellij (Android Studio). While I was at Google I/O this past week, I decided to stop by the JetBrains booth and talk to a couple of the reps about what Intellij offers, as I&rsquo;ll probably be switching, at least for Android development.</p>

<h3>Android Studio is Intellij 13</h3>

<p>From what they said, Android Studio is basically Intellij 13 (which is currently in preview mode), with some features removed or turned off. If all you want to do is Android development, Android Studio will be a good fit. If you want to also work on some server-side Java stuff, you might want Intellij Community Edition. If you also want things like JavaScript support, and other languages, you may want to opt for the $199 Ultimate.</p>

<h3>My workflow</h3>

<p>I like to play around with a lot of different languages and frameworks. I like trying new things, but I don&rsquo;t necessarily want to build large projects with them. I also don&rsquo;t want a dozen different IDEs to learn and to need to do big context switches between projects. Currently, for any non-java stuff, I spend most of my time in the command line, using vim, and managing workspaces with tmux. For Android and other Java projects, I do use Eclipse, and will probably switch over to Intellij. I have recently played around with IDEs for Clojure (LightTable) and Go (LiteIDE).</p>

<p>I&rsquo;m really not sure that I want to spend $199 (likely annually) for a tool that may or may not end up helping me more than the free version. I also don&rsquo;t want to have a dozen different IDEs to manage.</p>

<h3>Vim</h3>

<p>I decided that instead of leaning on a large IDE, I could probably get most of what I need from vim. I started googling around and found that there are plenty of plugins that will allow me to do code-completion in vim for most languages. There are a few other things that an IDE might buy me, but this is the biggest one. Since all I need is command line access, and this is already a major part of my workflow, this seems like a winner. I&rsquo;m hoping that by really learning vim, that I can escape the pull of most IDEs, and be more productive in the command line.</p>

<h3>My new setup</h3>

<p>Here&rsquo;s what I&rsquo;m installing:</p>

<script src="https://gist.github.com/emil10001/5609781.js?file=vimrc" type="text/javascript"></script><p>You might notice a comment in there above the YouCompleteMe bundle line, which says that you&rsquo;re going to need to likely compile and install vim from source to get this working. So, that&rsquo;s what I did.</p>

<h3>Compiling and installing vim</h3>

<p>This was a bit more difficult than the docs on YouCompleteMe&rsquo;s github page suggest. Mainly, because you&rsquo;re compiling against vim&rsquo;s development repository, you might run into a broken build. Luckily, it is rather simple to update to a different commit in the repo, recompile and try again. If you run into this issue, I suggest going to the repo yourself, and picking out a version to try, as opposed to using the one that I&rsquo;m supplying. There are no guarantees that the one that I picked is good for anything other than fixing the one issue that I ran into.</p>

<script src="https://gist.github.com/emil10001/5609781.js?file=install_vim.sh" type="text/javascript"></script><h3>Installing vundle</h3>

<p>Now, you can get vundle, and all your other bundles installed in one go:</p>

<script src="https://gist.github.com/emil10001/5609781.js?file=vundle_install.sh" type="text/javascript"></script><h3>Installing YouCompleteMe</h3>

<p>There&rsquo;s a component of YouCompleteMe that needs to be compiled and added. Here&rsquo;s how you do that:</p>

<script src="https://gist.github.com/emil10001/5609781.js?file=ycm_install.sh" type="text/javascript"></script><h3>Installing ctags</h3>

<p>Ctags also need to be compiled and installed:</p>

<script src="https://gist.github.com/emil10001/5609781.js?file=ctags_install.sh" type="text/javascript"></script><h3>Conclusion</h3>

<p>I know that I didn&rsquo;t go through all of the plugins that I chose. Maybe I&rsquo;ll do that in another post. We&rsquo;ll see. Getting this far on one box took me long enough, first researching, then working around the issues that I ran into with my initial attempts at a compiled vim. Already, the auto-complete provided by YouCompleteMe looks interesting, and I&rsquo;m excited to try out some of the functionality offered by the other plugins.</p>

<p>One nice thing is that all you need to do to get rid of any of these plugins is to simply delete the directory from ~/.vim/bundle/ and remove the bundle entry from your ~/.vimrc.</p>
