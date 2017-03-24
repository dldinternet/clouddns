#!/usr/bin/env rake
require 'rake'
require 'rake/testtask'
require 'bundler'
require 'bundler/gem_tasks'
require 'rubygems'
require 'rubygems/package_task'

task :default => [:test]

desc 'Run tests.'
Rake::TestTask.new(:test) do |t|
  t.libs << 'lib' << 'test'
  t.pattern = 'test/**/*_test.rb'
  t.verbose = true
end

require 'rubygems/tasks'
Gem::Tasks.new

require File.dirname(__FILE__) + '/lib/clouddns/version'
desc "Commit it, push it, build it, tag it and ship it"
task :ship => [:clobber_package,:clobber_rdoc,:gem] do
  sh("git add -A")
  sh("git commit -m 'Ship #{::Clouddns::VERSION}'")
  sh("git tag #{::Clouddns::VERSION}")
  sh("git push origin --tags")
  Dir[File.expand_path("../pkg/*.gem", __FILE__)].reverse.each do |built_gem|
    sh("gem push #{built_gem}")
  end
end

Dir[File.expand_path("../*gemspec", __FILE__)].reverse.each do |gemspec_path|
  gemspec = eval(IO.read(gemspec_path))
  Gem::PackageTask.new(gemspec).define
end
