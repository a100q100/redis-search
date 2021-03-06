require 'rake'
require 'rspec'
require 'bundler/setup'
Bundler::GemHelper.install_tasks

require File.expand_path('../spec/spec_helper', __FILE__)

namespace :benchmark do
  task :random_words do
    random = RandomWord.new
    Benchmark.bm do |bm|
      bm.report('Generate 1 random words') do
        1.times do |_i|
          name = random.next
        end
      end

      bm.report('Generate 100 random words') do
        100.times do |_i|
          name = random.next
        end
      end

      bm.report('Generate 10,000 random words') do
        10_000.times do |_i|
          name = random.next
        end
      end

      bm.report('Generate 100,000 random words') do
        100_000.times do |_i|
          name = random.next
        end
      end

      bm.report('Generate 1,000,000 random words') do
        1_000_000.times do |_i|
          name = random.next
        end
      end
    end
  end

  task :index do
    random = RandomWord.new
    Redis::Search::Index.new(type: 'CategoryTest',
                             title: random.next,
                             id: 0,
                             score: 1).save

    Benchmark.bm do |bm|
      bm.report('Index 1,000 items') do
        1_000.times do |i|
          Redis::Search::Index.new(type: 'CategoryTest',
                                   title: random.next,
                                   id: i,
                                   score: 1).save
        end
      end
    end
  end

  task :complete do
    keys_count = $redis.dbsize
    puts "Complete Benchmark from [CategoryTest], current have (#{keys_count} keys) in Redis"
    %w(c ca can jack).each do |q|
      puts "Search by [#{q}]"
      puts "    #{'-' * 90}"
      puts "    #{Redis::Search.complete('CategoryTest', q, limit: 10).collect { |c| [c['id'], c['title']].join(':') }}"
      puts "    #{'-' * 90}"
      puts "There have [#{Redis::Search.complete('CategoryTest', q, limit: 1_000_000).count}] items like '#{q}'"

      Benchmark.bm do |bm|
        bm.report('    1 times') do
          1.times do |_i|
            Redis::Search.complete('CategoryTest', q, limit: 10)
          end
        end

        bm.report('    10 times') do
          10.times do |_i|
            Redis::Search.complete('CategoryTest', q, limit: 10)
          end
        end

        bm.report('    100 times') do
          100.times do |_i|
            Redis::Search.complete('CategoryTest', q, limit: 10)
          end
        end
      end
      puts ''
    end
  end

  task :test1 do
    Benchmark.bm do |bm|
      puts "    c result:\n#{Redis::Search.complete('CategoryTest', 'c', limit: 10).collect { |c| c['title'] }}"
      bm.report("    'c' word 10 times") do
        10.times do |_i|
          Redis::Search.complete('CategoryTest', 'c', limit: 10)
        end
      end

      puts "    a result:\n#{Redis::Search.complete('CategoryTest', 'a', limit: 10).collect { |c| c['title'] }}"
      bm.report("    'a' word 10 times") do
        10.times do |_i|
          Redis::Search.complete('CategoryTest', 'a', limit: 10)
        end
      end

      puts "    b result:\n#{Redis::Search.complete('CategoryTest', 'b', limit: 10).collect { |c| c['title'] }}"
      bm.report("    'b' word 10 times") do
        10.times do |_i|
          Redis::Search.complete('CategoryTest', 'b', limit: 10)
        end
      end

      puts "    f result:\n#{Redis::Search.complete('CategoryTest', 'f', limit: 10).collect { |c| c['title'] }}"
      bm.report("    'f' word 10 times") do
        10.times do |_i|
          Redis::Search.complete('CategoryTest', 'f', limit: 10)
        end
      end

      puts "    t result:\n#{Redis::Search.complete('CategoryTest', 't', limit: 10).collect { |c| c['title'] }}"
      bm.report("    't' word 10 times") do
        10.times do |_i|
          Redis::Search.complete('CategoryTest', 't', limit: 10)
        end
      end

      puts "    d result:\n#{Redis::Search.complete('CategoryTest', 'd', limit: 10).collect { |c| c['title'] }}"
      bm.report("    'd' word 10 times") do
        10.times do |_i|
          Redis::Search.complete('CategoryTest', 'd', limit: 10)
        end
      end

      puts "    m result:\n#{Redis::Search.complete('CategoryTest', 'm', limit: 10).collect { |c| c['title'] }}"
      bm.report("    'm' word 10 times") do
        10.times do |_i|
          Redis::Search.complete('CategoryTest', 'm', limit: 10)
        end
      end
    end
  end

  task :query do
    keys_count = $redis.dbsize
    puts "Query Benchmark from [CategoryTest], current have (#{keys_count} keys) in Redis"
    %w(Cabalic Latcher Pectolite Jackal).each do |q|
      puts "Search by [#{q}]"
      puts "    #{'-' * 90}"
      puts "    #{Redis::Search.query('CategoryTest', q, limit: 10).collect { |c| [c['id'], c['title']].join(':') }}"
      puts "    #{'-' * 90}"
      puts "There have [#{Redis::Search.query('CategoryTest', q, limit: 1_000_000).count}] items like '#{q}'"

      Benchmark.bm do |bm|
        bm.report('    1 times') do
          1.times do |_i|
            Redis::Search.query('CategoryTest', q, limit: 10)
          end
        end

        bm.report('    10 times') do
          10.times do |_i|
            Redis::Search.query('CategoryTest', q, limit: 10)
          end
        end

        bm.report('    100 times') do
          100.times do |_i|
            Redis::Search.query('CategoryTest', q, limit: 10)
          end
        end
      end
      puts ''
    end
  end
end
