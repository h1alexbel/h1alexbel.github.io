# frozen_string_literal: true

require 'rubygems'
require 'rake'
require 'tempfile'
require 'rake/clean'
require 'scss_lint/rake_task'
require 'w3c_validators'
require 'nokogiri'
require 'rubocop/rake_task'
require 'English'
require 'net/http'
require 'html-proofer'
require 'mdl'

task default: %i[
  clean
  build
  scss_lint
  pages
  garbage
  orphans
  markdownlint
  rubocop
  spell
  w3c
  proofer
]

def done(msg)
  puts msg + "\n\n"
end

desc 'Delete _site directory'
task :clean do
  rm_rf '_site'
  done 'Jekyll site directory deleted'
end

desc 'Lint SASS sources'
SCSSLint::RakeTask.new do |t|
  # f = Tempfile.new(['main', '.scss'])
  f = File.new('main.scss', 'w')
  f << File.open('assets/css/main.scss').drop(2).join("\n")
  f.flush
  t.files = Dir.glob([f.path])
  # File.delete(f) if File.exist?(f)
  f.close
end

desc 'Build Jekyll site'
task :build do
  if File.exist? '_site'
    done 'Jekyll site already exists in _site'
  else
    system('jekyll build')
    raise 'Jekyll failed' unless $CHILD_STATUS.success?

    done 'Jekyll site generated without issues'
  end
end

desc 'Check the existence of all critical pages'
task pages: [:build] do
  File.open('_rake/pages.txt').map(&:strip).each do |p|
    file = "_site/#{p}"
    raise "Page #{file} is not found" unless File.exist? file

    puts "#{file}: OK"
  end
  done 'All files are in place'
end

desc 'Check the absence of garbage'
task garbage: [:build] do
  File.delete('main.scss') if File.exist?('main.scss')
  File.delete('_site/main.scss') if File.exist?('_site/main.scss')
  File.open('_rake/garbage.txt').map(&:strip).each do |p|
    file = "_site/#{p}"
    raise "Page #{file} is still there" if File.exist? file

    puts "#{file}: absent, OK"
  end
  done 'There is no garbage'
end

desc 'Validate a few pages for W3C compliance'
# It doesn't work now, because of: https://github.com/alexdunae/w3c_validators/issues/16
task w3c: [:build] do
  include W3CValidators
  validator = NuValidator.new
  [
    'index.html',
    '2018/07/27/props-file.html'
  ].each do |p|
    file = "_site/#{p}"
    results = validator.validate_file(file)
    unless results.errors.empty?
      results.errors.each do |err|
        puts err.to_s
      end
      raise "Page #{file} is not W3C compliant"

    end
    puts "#{p}: OK"
  end
  done 'HTML is W3C compliant'
end

desc 'Validate a few pages through HTML proofer'
task proofer: [:build] do
  HTMLProofer.check_directory(
    '_site',
    log_level: :warn,
    check_favicon: true,
    check_html: true
  ).run
  done 'HTML passed through html-proofer'
end

desc 'Check spelling in all HTML pages'
task spell: [:build] do
  Dir['_site/**/*.html'].each do |f|
    html = Nokogiri::HTML(File.read(f))
    html.search('//code').remove
    html.search('//script').remove
    html.search('//pre').remove
    html.search('//header').remove
    html.search('//footer').remove
    text = html.xpath('/html/body/section//p').text
    tmp = Tempfile.new(['bloghacks-', '.txt'])
    tmp << text
    tmp.flush
    tmp.close
    stdout = `cat "#{tmp.path}" \
      | aspell -a --lang=en_US -W 2 --ignore-case -p ./_rake/aspell.en.pws \
      | grep ^\\&`
    raise "Typos at #{f}:\n#{stdout}" unless stdout.empty?

    puts "#{f}: OK (#{text.split(/\s/).size} words)"
  end
  done 'No spelling errors'
end

desc 'Make sure there are no orphan articles'
task orphans: [:build] do
  links = Dir['_site/*/**/*.html'].reduce([]) do |array, f|
    array + Nokogiri::HTML(File.read(f)).xpath('//a/@href').to_a.map(&:to_s)
  end
  links = links
          .map { |a| a.gsub(%r{/^\//}, '/') }
          .select { |a| a.start_with? '/20' }
          .map { |a| a.gsub(/#.*/, '') }
  links += Dir['_site/*/**/*.html']
           .map { |f| f.gsub(/_site/, '') }
  counts = {}
  links
    .select { |a| a.match %r{.*/[0-9]{4}/[0-9]{2}/[0-9]{2}/.*} }
    .group_by(&:itself).each { |k, v| counts[k] = v.length }
  orphans = 0
  counts.each do |k, v|
    if v.zero?
      puts "#{k} is an orphan (#{v})"
      orphans += 1
    else
      puts "#{k}: #{v}"
    end
  end
  raise "There are #{orphans} orphans" unless orphans.zero?

  done "There are no orphans in #{links.size} links"
end

desc 'Run RuboCop on all Ruby files'
RuboCop::RakeTask.new do |t|
  t.fail_on_error = true
  t.requires << 'rubocop-rspec'
end

desc 'Markdown lint tool'
task :markdownlint do
  style = "#{__dir__}/style"
  sh "bundle exec mdl _posts --style #{style}"
end
