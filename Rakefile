def thread_pool
  @thread_pool ||= ThreadPool.new options.thread_pool_size
end

def pipe(command)
  output = ''
  IO.popen(command) do |io|
    until io.eof?
      buffer = io.gets
      output << buffer
      puts buffer
    end
  end

  output
end

task default: :serve

namespace :docs do
  task :index do
    if ENV["SITE_URL"] == 'https://www.braze.com' && ENV["RACK_ENV"] == 'production'
      puts `bundle exec jekyll algolia`
    end
  end
  task build: [:index] do
      puts `bundle exec jekyll build`
  end
  task :serve do
    pipe 'bundle exec jekyll s --port 5006'
  end
end

namespace :success do
  task :serve do
    pipe 'bundle exec ruby proxy.rb'
  end
end

namespace :assets do
  task :precompile do
    print 'no-op'
  end
end

multitask serve: [
  'docs:serve', 'success:serve'
]
