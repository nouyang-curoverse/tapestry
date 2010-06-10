# Add your own tasks in files placed in lib/tasks ending in .rake,
# for example lib/tasks/capistrano.rake, and they will automatically be available to Rake.

require(File.join(File.dirname(__FILE__), 'config', 'boot'))

require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'

require 'tasks/rails'

# http://rubyhitsquad.com/Vlad_the_Deployer.html
begin
  require 'vlad'
  Vlad.load :app => 'passenger'

  # http://withoutscope.com/2007/9/17/vlad-my-new-secret-love

  namespace "vlad" do
    desc "Setup the deploy target overrides"
    task :target

    rule "" do |task|
      if task.to_s.match(/vlad:target:([a-z_-]+)/i)
        override_path = File.join(RAILS_ROOT, 'config', "deploy.#{$1}.rb")
        Kernel.load override_path if File.exists? override_path
      end
    end

     desc "Copy config files from shared/config to release dir"
     remote_task :copy_config_files, :roles => :app do
       config_files.each do |filename|
         run "cp #{shared_path}/config/#{filename} #{release_path}/config/#{filename}"
       end
     end

     desc "Deploys"
     remote_task :deploy do
     end

    task :update do
      Rake::Task["vlad:copy_config_files"].invoke
    end

    task :deploy => [:update, :migrate, :start_app]
  end
rescue LoadError
  puts "Could not load ruby gem: vlad"
  # do nothing
end

