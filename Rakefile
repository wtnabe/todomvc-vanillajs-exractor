# -*- mode: ruby; coding: utf-8 -*-

require 'fileutils'
require 'rake'
require 'dotenv'

desc 'latest-todomvc-vanilla'
task 'latest-todomvc-vanilla' => ['clean:dest', 'todomvc:fetch'] do
  sh <<EOD
cp -r #{todomvc_dir} #{vanillajs_dir}
cd #{vanillajs_dir}
git filter-branch --subdirectory-filter examples/vanillajs
git remote add github #{vanillajs_repos}
git push -f github master
EOD
end

#
# [return] String
#
def todomvc_repos
  'https://github.com/tastejs/todomvc'
end

def vanillajs_repos
  "https://#{ENV['GITHUB_TOKEN']}@github.com/wtnabe/todomvc-vanillajs"
end

#
# [return] String
#
def todomvc_dir
  File.join(File.dirname(__FILE__), 'todomvc')
end

#
# [return] String
#
def vanillajs_dir
  File.join(File.dirname(__FILE__), 'vanillajs')
end

namespace :todomvc do
  desc 'todomvc:fetch'
  task :fetch do
    if File.exist?(todomvc_dir)
      sh <<EOD
cd #{todomvc_dir}
git pull
EOD
    else
      sh "git clone #{todomvc_repos}"
    end
  end
end

desc 'clean'
task :clean => ['clean:todomvc', 'clean:dest'] do; end

namespace :clean do
  desc 'clean:dest'
  task :dest do
    sh "rm -fr #{vanillajs_dir}"
  end

  desc 'clean:todomvc'
  task :todomvc do
    sh "rm -fr #{todomvc_dir}"
  end
end
