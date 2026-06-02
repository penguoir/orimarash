---
layout: post
title: "Advent of Code 2024 - Day 2: Red-Nosed Reports"
description: "Day 2 of the Advent of Code coding challenge. Restated problems: 1. You are given a list of \"records\". Each record is a list of numbers (called \"levels\"). How many of the records follow both these…"
date: 2024-12-02 23:26:16 +0000
image: /assets/posts/advent-of-code-2024-day-2-2/feature.jpg
tags: ["Advent of Code 2024"]
permalink: "/advent-of-code-2024-day-2-2/"
original_url: "https://www.orimarash.com/advent-of-code-2024-day-2-2/"
---

Day 2 of the Advent of Code coding challenge.

Restated problems:

1.  You are given a list of "records". Each record is a list of numbers (called "levels"). How many of the records follow both these conditions?
    1.  Monotonic (always increasing or always decreasing)
    2.  Levels change within a range 1-3
2.  Same as 1. but you can remove one number from the record before checking the conditions.

Solution:

```ruby
def each_way_to_remove_one_level(original_arr)
  original_arr.map.with_index do |_, index|
    original_arr.dup.tap { _1.delete_at(index) }
  end
end

def monotonic?(record)
  record.each_cons(2).map { |a, b| a <=> b }.uniq.count == 1
end

def small_distances?(record)
  record.each_cons(2).all? do |a, b|
    (a - b).abs.between?(1, 3)
  end
end

def safe?(record)
  monotonic?(record) && small_distances?(record)
end

def read_records_from_stdin
  ARGF.read.lines.map do |line|
    line.split.map(&:to_i)
  end
end

# Level 1
puts read_records_from_stdin.count { |record| safe?(record) }

# Level 2
puts read_records_from_stdin.count { |record|
  each_way_to_remove_one_level(record).any? { |record| safe?(record) }
}
```
