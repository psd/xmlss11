require "rubygems"
require "bundler/setup"

require 'nokogiri'
require 'albino'

OUTPUT_LIST = FileList['input/*.html'].pathmap('slides/%f')

task :default => OUTPUT_LIST

OUTPUT_LIST.each do |output_file|
  input_file = "input/#{File.basename(output_file)}"
  file output_file => [input_file] do
    puts "Processing: #{File.basename(output_file)}"
    doc = Nokogiri::XML(File.open(input_file))
    doc.css("pre").each do |node|
      if node.has_attribute?('data-lang')
        language = node.attr('data-lang')
        node.replace(Albino.colorize(node.inner_html, language.to_sym))
      end
    end
    File.open(output_file, 'w') do |f|
      f.write(doc.to_html)
    end
  end
end

