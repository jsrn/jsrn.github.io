require 'date'
require 'active_support/core_ext'

def end_of_month_timestamp
  Time.now.end_of_month.strftime('%Y-%m-%d')
end

def current_year
  Time.now.strftime('%Y')
end

def month_name
  Date::MONTHNAMES[Date.today.month]
end

def draft_path
  "_drafts/#{end_of_month_timestamp}-#{month_name.downcase}-#{current_year}.md"
end

task :draft_notes do
  puts '🌝 Generating empty monthnotes.'
  template = File.read('_drafts/monthnotes_template.md')
  template.gsub!('MONTH', month_name)
  template.gsub!('YEAR', current_year)

  File.open(draft_path, 'w') do |file|
    file.write(template)
  end
end

task :draft_post, [:title] do |t, args|
  args.with_defaults(title: 'Empty Post')
  puts 'Making post with title: ' << args.title
  # TODO: do
end