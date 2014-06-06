desc 'create new post'
# rake new title="New post title goes here" author="nishant"
task :new_post do
  require 'rubygems'
  title = ENV["title"] || "New Title"
  author = ENV["author"] || ""
  slug = title.gsub(' ','-').downcase || title.gsub(' ','-').downcase

  TARGET_DIR = "_posts"

  filename = "#{Time.new.strftime('%Y-%m-%d')}-#{slug}.markdown"
  path = File.join(TARGET_DIR, filename)
  post = <<-HTML
---
layout: post
title: "TITLE"
date: DATE
author: "AUTHOR"
---

  HTML
  post.gsub!('TITLE', title).gsub!('DATE', Time.new.to_s).gsub!('AUTHOR', author)
  File.open(path, 'w') do |file|
    file.puts post
  end
  puts "new post generated in #{path}"
end
