require 'rake'
require 'yaml'
require 'fileutils'
require 'date'

default_site_configuration = './_config.yml'

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

task :s => :serve
task :new_post => :create_new_post
task :new_page => :create_new_page

# Method to preview the site
desc 'Preview the page locally'
task :serve do
  system('bundle exec jekyll serve')
end

# Method to check the site for failing links
desc 'Check page for dead links'
task :check_links do
  config = YAML.load_file(default_site_configuration)
  puts `bundle exec htmlproofer #{config['destination']} + '/'`

end

# Method to write any content to any location and
# create the required path.
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

# Deploy
desc 'Deploy everything'
task :deploy do
  puts 'Committing'
  puts `git commit -am 'General update'`
  puts 'Uploading to repository.'
  puts `CURRENTBRANCH=$(git status | grep -m 1 branch | awk '\''{ print $3 }'\'') && git push origin ${CURRENTBRANCH}`

end

# Create pages
desc 'Create a new page at the given url'
task :create_new_page, :url do |t,args|
  puts 'Creating a new page at ' + args[:url] + '.md'
  url = args[:url] + '.md'

  # Replace placeholders in default content
  new_content = default_page_content.gsub(/\#\#title\#\#/, args[:url].split('/').last.capitalize)
  new_content.gsub!(/\#\#url\#\#/, args[:url] + '/')
  write_content(url,new_content)
end

# Create posts
desc 'Create a new post with an optional title.'
task :create_new_post, :title do |t,args|
  post_dir = './_posts'
  args.with_defaults(:title => 'Insert title here')

  # Current date
  daystamp = Time.now.strftime("%Y-%m-%d")
  timestamp = Time.now.strftime("%Y-%m-%d %H:%M:%S +0100")

  post_filename = post_dir + '/' + daystamp + '-' + args[:title].gsub(/ /,'_').downcase + '.md'

  if not File.exists?(post_filename)
    puts 'Creating a new post at ' + post_filename
    new_content = default_post_content.gsub(/\#\#title\#\#/, args[:title].capitalize)
    new_content.gsub!(/\#\#date\#\#/, timestamp)
    write_content(post_filename, new_content)
  else
    puts 'Post ' + post_filename + ' already exists. Abort.'
  end

end

task :default do
  system "rake --tasks"
end
