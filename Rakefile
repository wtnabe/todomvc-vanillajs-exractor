# -*- mode: ruby; coding: utf-8 -*-

require 'fileutils'
require 'rake'
require 'dotenv'
require 'pp'
require 'json'

desc 'latest-todomvc-vanilla'
task 'latest-todomvc-vanilla' => ['clean:dest', 'todomvc:add-eslint'] do
  sh <<EOD
cd #{vanillajs_dir}
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

  desc 'extract vanillajs version'
  task :'extract' => [:fetch] do
    sh <<EOD
cp -r #{todomvc_dir} #{vanillajs_dir}
cd #{vanillajs_dir}
git filter-branch --subdirectory-filter examples/vanillajs
EOD
  end

  desc 'add eslint and config and scripts'
  task :'add-eslint' => [:extract] do
    eslint = {
      devDependencies: {
        eslint: "^2.9.0",
        esprima: "^2.7.2"
      },
      append: {
        eslintConfig: {
          parser: "esprima",
          rules: {
            semi: "error"
          }
        },
        scripts: {
          test: "eslint --quiet js"
        }
      }
    }

    package_json = File.join(vanillajs_dir, 'package.json')

    npm_config = JSON.parse(open(package_json).read)

    npm_config["devDependencies"].merge!(eslint[:devDependencies])
    npm_config.merge!(eslint[:append])

    open(package_json, 'w') {|f|
      f.puts JSON.pretty_generate(npm_config)
    }

    sh <<EOD
cd #{vanillajs_dir}
git commit -a -m 'add eslint and config and scripts'
EOD
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
