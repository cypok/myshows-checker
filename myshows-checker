#!/usr/bin/env ruby

require 'rubygems'
require 'myshows'

def msg(text); puts text end
def err(text); puts "Error: #{text}" end

def parse_filename(filename)
  sep = /[. _]/
  filename =~ /^(.*)#{sep}s(\d{1,2})#{sep}?e(\d{1,2})#{sep}/i or
  filename =~ /^(.*)#{sep}-#{sep}(\d{1,2})x(\d{1,2})#{sep}/i or
  filename =~ /^(.*)#{sep}(\d{1,2})x(\d{1,2})#{sep}/i or
  raise "unrecognized series filename <#{filename}>"

  name = $1
  season = $2
  episode = $3
  [name.gsub(sep, ' '), season.to_i, episode.to_i]
end

begin
  f = File.open(File.expand_path('~/.myshows'), 'r')
  profile = MyShows::Profile.new f.gets.strip, f.gets.strip
  f.close
rescue Exception => e
  err e.message
  exit 1
end

ARGV.each do |path|
  begin
    show_title, season_num, episode_num = parse_filename(File.basename(path))

    show = profile.show(show_title)
    ep = show.episode(season_num, episode_num)
    ep.check!

    msg "Checked #{ep}"
  rescue Exception => e
    err e.message
    next
  end
end

