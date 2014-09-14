# encoding: utf-8
require 'puppet-lint/tasks/puppet-lint'
require 'rspec/core/rake_task'

PuppetLint.configuration.ignore_paths = ["librarian/**/*.pp"]

TESTED_MODULES = %w(mysql)
namespace :spec do
  TESTED_MODULES.each do |module_name|
    desc "Test from module #{module_name}"
    RSpec::Core::RakeTask.new(module_name) do |t|
      t.pattern = "modules/#{module_name}/spec/**/*_spec.rb"
    end
  end
end

desc "All tests"
task :spec => TESTED_MODULES.map {|m| "spec:#{m}" }

namespace :librarian do
  desc "Install Librarian Puppet modules"
  task :install do
    Dir.chdir('librarian') do
      sh "librarian-puppet install"
	end 
   end
end

desc "Create package puppet.tgz"
task :package => ['librarian:install', :lint, :spec] do
  sh "tar czvf puppet.tgz manifests modules librarian/modules"
end

desc "Remove package puppet.tgz"
task :clean do
  sh "rm puppet.tgz"
end

task :default => [:spec]