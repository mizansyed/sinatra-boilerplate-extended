require 'rake/testtask'
require 'open-uri'
require 'fileutils'
require 'zip/zipfilesystem'

Rake::TestTask.new do |t|
  t.libs.push("./")
  t.test_files = FileList['tests/*.rb']
  t.verbose = true
end

desc "Compiles all .scss files in the sass directory and places them in public/css (minified)"
task :sass do
  puts "Compiling and minifying .scss files in sass/"
  scss_files = FileList['sass/*.scss']
  scss_files.each do |file|
    puts "Processing: #{file}"
    `sass #{file}:public/css/#{file[5..-5]}css --style compressed`
  end
end

namespace :update do
  #JQUERY STUFF
  desc "Download the latest version of JQuery (minified)"
  task :jquery do
    open('jquery.js', 'wb') do |file|
      file << open('http://code.jquery.com/jquery-latest.min.js').read
    end
    FileUtils.mv('./jquery.js', './public/js/jquery.js')
    puts "Downloaded the latest JQuery"
  end


  #BOOTSTRAP STUFF
  desc "Download the latest version of Twitter Bootstrap (minified)"
  task :bootstrap do
    open('bootstrap.zip', 'wb') do |file|
      file << open('http://getbootstrap.com/2.3.2/assets/bootstrap.zip').read
    end

    Zip::ZipFile.open("bootstrap.zip") do |zipfile|
      zipfile.each do |file|
        zipfile.extract(file, "./#{file.name}")
      end
    end

    FileUtils.mv('./bootstrap/css/bootstrap.min.css', './public/css/bootstrap.min.css')
    FileUtils.mv('./bootstrap/img/glyphicons-halflings-white.png', './public/img/glyphicons-halflings-white.png')
    FileUtils.mv('./bootstrap/img/glyphicons-halflings.png', './public/img/glyphicons-halflings.png')
    FileUtils.mv('./bootstrap/js/bootstrap.min.js', './public/js/bootstrap.min.js')
    FileUtils.rm_r('./bootstrap')
    FileUtils.rm('./bootstrap.zip')
    Rake::Task["update:bootstraptemplate"].invoke
    puts "Downloaded the latest Twitter Bootstrap"
  end

  desc "Write a Bootstrap base template file"
  task :bootstraptemplate do
    File.open("./views/layouts/layout.erb", "w") do |f|     
      f.write(bootstrap_template)   
    end
  end

  desc "Downloads the most recent versions of JQuery & Twitter Bootstrap"
  task :allbootstrap do
    Rake::Task["update:jquery"].invoke
    Rake::Task["update:bootstrap"].invoke
  end


  #SKELETON STUFF
  desc "Downloads Skeleton Framework"
  task :skeleton do
    open('skeleton.zip', 'wb') do |file|
      file << open('https://github.com/dhg/Skeleton/archive/master.zip').read
    end

    Zip::ZipFile.open("skeleton.zip") do |zipfile|
      zipfile.each do |file|
        zipfile.extract(file, "./#{file.name}")
      end
    end

    FileUtils.mv('./Skeleton-master/stylesheets/base.css', './public/css/base.css')
    FileUtils.mv('./Skeleton-master/stylesheets/layout.css', './public/css/layout.css')
    FileUtils.mv('./Skeleton-master/stylesheets/skeleton.css', './public/css/skeleton.css')
    FileUtils.rm_r('./Skeleton-master')
    FileUtils.rm('./skeleton.zip')
    Rake::Task["update:skeletontemplate"].invoke
    puts "Downloaded the latest Skeleton Framework"
  end

  desc "Write a Skeleton Framework base template file"
  task :skeletontemplate do
    File.open("./views/layouts/layout.erb", "w") do |f|     
      f.write(skeleton_template)   
    end
  end

  desc "Downloads the most recent versions of JQuery & Skeleton"
  task :allskeleton do
    Rake::Task["update:jquery"].invoke
    Rake::Task["update:skeleton"].invoke
  end
end


#REMOVAL
namespace :remove do
  desc "Remove JQuery from the project"
  task :jquery do
    FileUtils.rm('./public/js/jquery.js') if File.file?('./public/js/jquery.js')
  end

  desc "Remove Twitter Bootstrap from the project"
  task :bootstrap do
    FileUtils.rm('./public/css/bootstrap.min.css')  if File.file?('./public/css/bootstrap.min.css')
    FileUtils.rm('./public/img/glyphicons-halflings-white.png')  if File.file?('./public/img/glyphicons-halflings-white.png')
    FileUtils.rm('./public/img/glyphicons-halflings.png')  if File.file?('./public/img/glyphicons-halflings.png')
    FileUtils.rm('./public/js/bootstrap.min.js')  if File.file?('./public/js/bootstrap.min.js')
  end

  desc "Remove Skeleton Framework from the project"
  task :skeleton do
    FileUtils.rm('./public/css/base.css') if File.file?('./public/css/base.css')
    FileUtils.rm('./public/css/layout.css') if File.file?('./public/css/layout.css')
    FileUtils.rm('./public/css/skeleton.css') if File.file?('./public/css/skeleton.css')
  end

  desc "Remove both JQuery & Twitter Bootstrap or Skeleton Framework from the project"
  task :all do
    Rake::Task["remove:jquery"].invoke
    Rake::Task["remove:bootstrap"].invoke
    Rake::Task["remove:skeleton"].invoke
  end
end

desc "List all routes for this application"
task :routes do
  puts `grep '^[get|post|put|delete].*do$' app/controllers/*.rb | sed 's/ do$//'`
end

task :default => ["test"]




def skeleton_template

<<-HTML
<!DOCTYPE html>
<!--[if lt IE 7 ]><html class="ie ie6" lang="en"> <![endif]-->
<!--[if IE 7 ]><html class="ie ie7" lang="en"> <![endif]-->
<!--[if IE 8 ]><html class="ie ie8" lang="en"> <![endif]-->
<!--[if (gte IE 9)|!(IE)]><!--><html lang="en"> <!--<![endif]-->
<head>
  <meta charset="utf-8">
  <title><%= render_partial :title %></title>
  <link rel="stylesheet" href="/css/base.css">
  <link rel="stylesheet" href="/css/skeleton.css">
  <link rel="stylesheet" href="/css/layout.css">
  <link rel="stylesheet" href="/css/style.css">
  <!--[if lt IE 9]>
    <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
  <![endif]-->
</head>
<body>
<div class="container">
  <div class="sixteen columns"</div>
        <h1><%= render_partial :title %></h1>
        <%= yield %>
  </div>
</div>
</body>
</html>
HTML

end


def bootstrap_template

<<-HTML
<!DOCTYPE HTML>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title><%= render_partial :title %></title>
    <link rel="stylesheet" href="/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/style.css">
  </head>
  
  <body>
    <div class="container">
      <div class="row">
        <h1><%= render_partial :title %></h1>
        <%= yield %>
      </div>
    </div>
  </body>
</html>
HTML

end