This project reproduces a problem with trinidad_resque_extension.  Here's what happens
with the code in this branch:

	$ rails s trinidad
	
Web server starts up fine, but resque never gets loaded.

If we run it like this:

	$ trinidad
	
We get this error:

	NoMethodError: undefined method `register' for Rack::Handler:Module

Which causes the web server to fail booting.  Then it's followed by this error:

	undefined method `require_dependency' for #<Blog::Application:0x6b38c54e>

	Tasks: TOP => resque:work => resque:preload
	
which causes the resque workers to fail booting.

If we remove all trinidad gems from the Gemfile, we get this:

	$ trinidad
	NOTE: Gem::GemPathSearcher#initialize is deprecated with no replacement. It will be removed on or after 2011-10-01.
	Gem::GemPathSearcher#initialize called from /Users/jkutner/.rvm/gems/jruby-1.6.5/gems/trinidad_resque_extension-0.1.0/lib/trinidad_resque_extension.rb:51.
	NOTE: Gem::GemPathSearcher#find is deprecated with no replacement. It will be removed on or after 2011-10-01.
	Gem::GemPathSearcher#find called from /Users/jkutner/.rvm/gems/jruby-1.6.5/gems/trinidad_resque_extension-0.1.0/lib/trinidad_resque_extension.rb:51.
	Jan 12, 2012 4:41:17 PM org.apache.coyote.AbstractProtocol init
	INFO: Initializing ProtocolHandler ["http-bio-3000"]
	Jan 12, 2012 4:41:17 PM org.apache.catalina.core.StandardService startInternal
	INFO: Starting service Tomcat
	Jan 12, 2012 4:41:17 PM org.apache.catalina.core.StandardEngine startInternal
	INFO: Starting Servlet Engine: Apache Tomcat/7.0.23
	Invalid log level , using default: INFO
	2012-01-12 22:41:17 INFO: No global web.xml found
	2012-01-12 22:41:19 INFO: jruby 1.6.5 (ruby-1.8.7-p330) (2011-10-25 9dcd388) (Java HotSpot(TM) 64-Bit Server VM 1.6.0_29) [darwin-x86_64-java]
	:public is no longer used to avoid overloading Module#public, use :public_folder instead
		from /Users/jkutner/.rvm/gems/jruby-1.6.5/gems/resque-1.19.0/lib/resque/server.rb:12:in `Server'
	2012-01-12 22:41:22 INFO: The start() method was called on component [Realm[Simple]] after start() had already been called. The second call will be ignored.
	2012-01-12 22:41:23 INFO: Info: received max runtimes = 5
	2012-01-12 22:41:23 INFO: jruby 1.6.5 (ruby-1.8.7-p330) (2011-10-25 9dcd388) (Java HotSpot(TM) 64-Bit Server VM 1.6.0_29) [darwin-x86_64-java]
	2012-01-12 22:41:23 INFO: Info: using runtime pool timeout of 30 seconds
	2012-01-12 22:41:23 INFO: Info: received min runtimes = 1
	2012-01-12 22:41:23 INFO: Info: received max runtimes = 5
	rake aborted!
	Don't know how to build task 'resque:work'
	
	(See full trace by running task with --trace)
	2012-01-12 22:41:33 INFO: Info: add application to the pool. size now = 1
	2012-01-12 22:41:33 INFO: Starting ProtocolHandler ["http-bio-3000"]

Still not resque workers.  