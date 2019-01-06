require 'rake'
require 'yaml'
require 'fileutils'

default_page_content ="---\n" \
  "layout: page\n"\
  "title: '##title##'\n"\
  "permalink: ##url##\n"\
  "---\n"\
  ""

default_post_content = "---\n"\
  "layout: article\n"\
  "title: ##title##\n"\
  "date: ##date##\n"\
  "categories: \n"\
  "---\n"\
  ""

def write_content(path,content)
  # Split and remove empty fields
  path_elements = path.split('/').reject(&:empty?)
  filename = path_elements.last
  path_elements.pop(1) # Drop the filename
  dirname = path_elements.join('/')

  # Create the directories is needed
  FileUtils.mkdir_p(dirname) unless File.exists?(dirname)

  # Create the file if needed
  if not File.exists?(dirname + '/' + filename)
    puts 'Creating: ' + dirname + '/' + filename
    File.open(dirname + '/' + filename, 'w'){
      |file| file.write(content)
    }
  end

end

# Create pages
desc 'Create a new page at the given url'
task :create_new_page, :url do |t,args|
  puts 'Creating a new page at ' + args[:url] + '.md'
  url = args[:url] + '.md'

  # Replace placeholders in default content
  new_content = default_page_content.gsub(/\#\#title\#\#/, args[:url].split('/').last.capitalize)
  new_content.gsub!(/\#\#url\#\#/, args[:url] + '/')
  puts new_content
  write_content(url,new_content)
end

task :default do
  system "rake --tasks"
end
