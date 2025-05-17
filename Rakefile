require "rubygems"
require "tmpdir"

require "bundler/setup"
require "jekyll"


# Change your GitHub reponame eg. "kippt/jekyll-incorporated"
GITHUB_REPONAME = "id-ruby/id-ruby"
GH_PAGES_TOKEN = ENV["GH_PAGES_TOKEN"]


namespace :site do
  desc "Generate blog files"
  task :generate do
    Jekyll::Site.new(Jekyll.configuration({
      "source"      => ".",
      "destination" => "_site"
    })).process
  end


  desc "Generate and publish blog to gh-pages"
  task :publish => [:generate] do
    current_directory = Dir.pwd

    Dir.mktmpdir do |tmp|
      cp_r "_site/.", tmp
      Dir.chdir tmp
      system "git init"
      system "git config user.name id-ruby"
      system "git config user.email team@perkodi.org"
      system "git add ."
      message = "Site updated at #{Time.now.utc}"
      system "git commit -m #{message.inspect}"
      system "git remote add origin #{git_origin}"
      system "git push origin master:refs/heads/gh-pages --force"
      Dir.chdir current_directory
    end
  end

  def git_origin
    if GH_PAGES_TOKEN
      "https://x-access-token:#{GH_PAGES_TOKEN}@github.com/#{GITHUB_REPONAME}.git"
    else
      "git@github.com:#{GITHUB_REPONAME}.git"
    end
  end
end
