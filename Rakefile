require 'date'
require 'active_support/core_ext'

def end_of_month_timestamp
  Time.now.end_of_month.strftime('%Y-%m-%d')
end

def current_timestamp
  Time.now.strftime('%Y-%m-%d')
end

def current_year
  Time.now.strftime('%Y')
end

def month_name
  Date::MONTHNAMES[Date.today.month]
end

def draft_note_path
  "_drafts/#{end_of_month_timestamp}-#{month_name.downcase}-#{current_year}.md"
end

def draft_post_path(name)
  formatted_name = name.downcase.gsub(' ', '-').gsub(/[^A-Za-z0-9\-]/, '')
  "_drafts/#{current_timestamp}-#{formatted_name}.md"
end

task :draft_notes do
  puts '🌝 Generating empty monthnotes.'
  template = File.read('_drafts/monthnotes_template.md')
  template.gsub!('MONTH', month_name)
  template.gsub!('YEAR', current_year)

  File.open(draft_note_path, 'w') do |file|
    file.write(template)
  end
end

task :draft_post, [:title] do |_t, args|
  puts '🌝 Generating empty blog post.'
  args.with_defaults(title: 'Empty Post')

  template = File.read('_drafts/blogpost_template.md')
  template.gsub!('TITLE', args.title)

  File.open(draft_post_path(args.title), 'w') do |file|
    file.write(template)
  end
end
