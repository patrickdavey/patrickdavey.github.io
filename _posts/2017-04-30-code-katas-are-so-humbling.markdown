--- 
wordpress_id: PSD-17
layout: post
title: Code katas are so humbling
---

I have been working through, slowly, the ruby track on [exercism](http://exercism.io). It's a great site which I highly reccommend. Anyway, todays task was to identify saddle points. Here... I'll print the readme

### Saddle point README

```plain
# Saddle Points

Detect saddle points in a matrix.

So say you have a matrix like so:

    0  1  2
  |---------
0 | 9  8  7
1 | 5  3  2     <--- saddle point at (1,0)
2 | 6  6  7

It has a saddle point at (1, 0).

It's called a "saddle point" because it is greater than or equal to
every element in its row and the less than or equal to every element in
its column.

A matrix may have zero or more saddle points.

Your code should be able to provide the (possibly empty) list of all the
saddle points for any given matrix.
```

You are then given a spec file

```ruby

#!/usr/bin/env ruby
gem 'minitest', '>= 5.0.0'
require 'minitest/autorun'
require_relative 'saddle_points'

class MatrixTest < Minitest::Test
  def test_extract_a_row
    matrix = Matrix.new("1 2\n10 20")
    assert_equal [1, 2], matrix.rows[0]
  end

  def test_extract_same_row_again
    matrix = Matrix.new("9 7\n8 6")
    assert_equal [9, 7], matrix.rows[0]
  end

  def test_extract_other_row
    matrix = Matrix.new("9 8 7\n19 18 17")
    assert_equal [19, 18, 17], matrix.rows[1]
  end

  def test_extract_other_row_again
    matrix = Matrix.new("1 4 9\n16 25 36")
    assert_equal [16, 25, 36], matrix.rows[1]
  end

  def test_extract_a_column
    matrix = Matrix.new("1 2 3\n4 5 6\n7 8 9\n 8 7 6")
    assert_equal [1, 4, 7, 8], matrix.columns[0]
  end

  def test_extract_another_column
    matrix = Matrix.new("89 1903 3\n18 3 1\n9 4 800")
    assert_equal [1903, 3, 4], matrix.columns[1]
  end

  def test_no_saddle_point
    matrix = Matrix.new("2 1\n1 2")
    assert_equal [], matrix.saddle_points
  end

  def test_a_saddle_point
    matrix = Matrix.new("1 2\n3 4")
    assert_equal [[0, 1]], matrix.saddle_points
  end

  def test_another_saddle_point
    matrix = Matrix.new("18 3 39 19 91\n38 10 8 77 320\n3 4 8 6 7")
    assert_equal [[2, 2]], matrix.saddle_points
  end

  def test_multiple_saddle_points
    matrix = Matrix.new("4 5 4\n3 5 5\n1 5 4")
    assert_equal [[0, 1], [1, 1], [2, 1]], matrix.saddle_points
  end
end
```

And then you have to give your own solution.

Mine was, frankly, horrible. I was slightly stymied as I had to keep the `rows` and `column` methods as they were. I ended up making a terribly verbose class, creating a `Point` class etc.

I knew it was horrible too, the `@rows` shadowing the  `rows` method (which returned values), it was horrible. If I'd been able to refactor the tests I think I could have cleared it up quite a bit more nicely. I did like my little `Point` value object though, that was nice.

## My solution

```ruby

class Matrix
  def initialize(matrix_string)
    @rows = matrix_string
            .lines
            .map(&:strip)
            .map(&:split)
            .map { |row| row.map(&:to_i) }

    @rows = @rows.map.with_index do |cols, row_index|
      cols.map.with_index do |value, col_index|
        Point.new(row_index, col_index, value)
      end
    end

    @columns = @rows.transpose
  end

  def saddle_points
    max_row_elements.select do |point|
      col_for_point = @columns[point.col]
      col_for_point.all? { |p| (point.value <= p.value)  || p == point } ? point : nil
    end.flatten.compact.map(&:to_coordinates)
  end

  def rows
    @rows.map do |row|
      row.map(&:value)
    end
  end

  def columns
    @columns.map do |column|
      column.map(&:value)
    end
  end

  private

  def max_row_elements
    @max_row_elements ||= begin
      @rows.map do |row|
        max_value_in_row = row.max.value
        row.select {|p| p.value == max_value_in_row}
      end
    end.flatten
  end

  class Point < Struct.new(:row, :col, :value)
    def <=>(other)
      value <=> other.value
    end

    def ==(other)
      row == other.row && col == other.col
    end

    def to_coordinates
      [row, col]
    end
  end
end
```

Anyway, after looking at other peoples solutions, I saw that the proper way to do it was to just neatly iterate over all the positions in the Matrix, and that the easiest way to do that was to use the `each_index` method for the rows & columns combined with the `product` method.

Obvious in retrospect.. looks like this:

```ruby

class Matrix
  attr_reader :rows, :columns

  def initialize(string)
    @rows = string.each_line.map { |line| line.split.map(&:to_i) }
    @columns = @rows.transpose
  end

  def saddle_points
    all_coordinates.select do |i, j|
      rows[i].max == rows[i][j] && columns[j].min == rows[i][j]
    end
  end


  private

  def all_coordinates
    rows.each_index.to_a.product(columns.each_index.to_a)
  end
end
```

Really quite a lot nicer, and infinitely shorter.


Well, I highly recommend [exercism](http://exercism.io), it's really good.
