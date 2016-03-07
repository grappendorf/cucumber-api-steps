require 'bundler/gem_tasks'

require 'cucumber/rake/task'

Cucumber::Rake::Task.new do |t|
  t.cucumber_opts = %w{--format pretty}
end

task :default => :cucumber

task :build do
  require 'rubygems/gem_runner'
  gemspec = eval(File.read 'cucumber-api-steps.gemspec')
  Gem::GemRunner.new.run ['build', "#{gemspec.name}.gemspec"]
end

task :release_local => :build do
  require 'geminabox_client'
  require 'yaml'
  gemspec = eval(File.read 'cucumber-api-steps.gemspec')
  rake_config = YAML::load(File.read("#{ENV['HOME']}/.rake/rake.yml")) rescue {}
  GeminaboxClient.new(rake_config['geminabox']['url']).push "#{gemspec.name}-#{gemspec.version}.gem", overwrite: true
  puts "Gem #{gemspec.name} pushed to #{rake_config['geminabox']['url']}"
end
