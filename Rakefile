task :dev do
  sh "bundle exec jekyll serve --watch"
end

task :build do
  puts '------------------'
  puts 'build jekyll site'
  puts '------------------'

  sh "bundle exec jekyll build"
end

desc 'Deploy site to kakutame.com'
task :deploy => [:build] do
  puts '------------------'
  puts 'deploy start'
  puts '------------------'

  require 'net/ssh'
  require 'net/sftp'
  require 'dotenv'

  Dotenv.load

  public_dir = "#{ENV['SERVER_HOME']}/#{ENV['SERVER_PUBLIC_DIR']}"

  Net::SSH.start(ENV['SERVER_HOST'], ENV['SERVER_USER'], port: ENV['SERVER_PORT'], password: ENV['SERVER_KEY']) do |ssh|
    if ssh.sftp.dir.entries("#{ENV['SERVER_HOME']}").map { |e| e.name }.include?(ENV['SERVER_PUBLIC_DIR'])
      puts 'Cleaning available files...'
      ssh.exec!("rm -rf #{public_dir}")
      puts 'Done'
    end

    puts 'Uploading site files...'
    ssh.sftp.mkdir!(public_dir)
    ssh.sftp.upload!('./_site', public_dir)

    puts 'Done'
  end
end
