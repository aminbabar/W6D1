# Partner B: White Boarding

Ask your partner the below questions! **Don't spend longer than 20 minutes per
coding question.** Start a timer once the question has finished being read (or
the relevant information has been presented).

## Question One

What are the disadvantages of adding an index to a table column in a database?

Create/Update would take more time, but returns a quicker lookup time.

**Answer**: Indices do have a cost. They make writes (`INSERT`s, `DELETE`s, and
`UPDATE`s) a little more taxing because the index table must also be updated.

## Question Two

Given the following table write all the `belongs_to` and `has_many` associations
for models based on the below table (`User`, `Enrollment`, and `Course`)

```ruby

# == Schema Information
#
# Table name: users
#
#  id   :integer                not null, primary key
#  name :string                 not null


# Table name: enrollments
#
#  id   :integer                not null, primary key
#  course_id :integer           not null
#  student_id :integer          not null


# Table name: courses
#
#  id   :integer                not null, primary key
#  course_name :string          not null
#  professor_id :integer        not null
#  prereq_course_id :integer    not null
```

class Enrollments

  belongs_to :course,
  primary_key: :id,
  foreign_key: :course_id,
  class_name: :Course

  belongs_to :student,
  primary_key: :id,
  foreign_key: :student_id,
  class_name: :User

end

class Course

  has_many :enrollments,
  primary_key: :id,
  foreign_key: :course_id
  class_name: :Enrollment

  belongs_to :professor,
  primary_key: :id,
  foreign_key: :professor_id
  class_name: :User

  belongs_to :prereq_course,
  primary_key: :id,
  foreign_key: :prereq_course_id
  class_name: Course

end

class User

  has_many :enrollments,
  primary_key: :id,
  foreign_key: :student_id
  class_name: :Enrollment

  has_many :courses,
  primary_key: :id,
  foreign_key: :professor_id
  class_name: :Course


end


### Solutions

Below are all the `belongs_to` and `has_many` associations:

```ruby
class Enrollment < ApplicationRecord
  belongs_to :user,
    class_name: 'User',
    foreign_key: :student_id,
    primary_key: :id

  belongs_to :course,
    class_name: 'Course',
    foreign_key: :course_id,
    primary_key: :id
end

class User < ApplicationRecord
  has_many :enrollments,
    class_name: 'Enrollment',
    foreign_key: :student_id,
    primary_key: :id

  has_many :courses_taught,
    class_name: 'Course',
    foreign_key: :professor_id,
    primary_key: :id,
    optional: true
end

class Course < ApplicationRecord
  has_many :enrollments,
    class_name: 'Enrollment',
    foreign_key: :course_id,
    primary_key: :id

  belongs_to :prerequisite,
    class_name: 'Course',
    foreign_key: :prereq_course_id,
    primary_key: :id,
    optional: true

  belongs_to :professor,
    class_name: 'User',
    foreign_key: :professor_id,
    primary_key: :id
end
```

## Question Three

Given all possible SQL commands order by order of query execution. (SELECT,
DISTINCT, FROM, JOIN, WHERE, GROUP BY, HAVING, LIMIT/OFFSET, ORDER).



1. FROM
2. JOIN
3. WHERE
4. GROUP BY
5. HAVING

<!-- X 6. ORDER -->

<!--  X 7. DISTINCT -->
8. SELECT
<!-- Distinct comes here -->
<!-- Order comes here -->
9. LIMIT/OFFSET

**Answer**:

1. FROM and JOINs
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. DISTINCT
7. ORDER BY
8. LIMIT / OFFSET

## Question Four

Given the following table:

```ruby
# == Schema Information
#
# Table name: nobels
#
#  yr          :integer
#  subject     :string
#  winner      :string

```

Write the following SQL Query:

1.  In which years was the Physics prize awarded, but no Chemistry prize?

SELECT yr
FROM nobels
GROUP BY yr
HAVING subject = 'Physics' AND subject != 'Chemistry'


#### Solution

```sql
SELECT DISTINCT
    yr
FROM
    nobels
WHERE
    (subject = 'Physics' AND yr NOT IN 
    (
    SELECT
        yr
    FROM
        nobels
    WHERE
        subject = 'Chemistry'
    ))


```

## Question Five

What is the purpose of a database migration?

A database migration creates and updates tables and their constraints.


**Answer**: A migration is a file containing Ruby code that describes a set of
changes to be applied to a database. It may create or drop tables as well as add
or remove columns from a table.

## Question Six

What is the difference between Database Constraints and Active Record
Validations?

DB constraints are within the DB level, they lead to bad errors that cannot be parsed.
Validations lead to better errors.
If two users try to create an account at the same time, validations might skip this error, but constraints would catch.


**Answer**: **Validations** are defined inside **models**. Model-level
validations live in the Rails world. Since we write them in Ruby, they are very
flexible, database agnostic, and convenient to test, maintain and reuse.
Validations are run whenever we `save` or `update` a model. **Constraints** are
defined inside **migrations** and operate on the database. Constraints throw
hard nasty errors whenever something that shouldn't be inputted into our
database tries to get in there.

## Question Seven

Given the following table write all the `belongs_to` and `has_many` associations
for models based on the below table (`User`, `Game`, and `Score`)

```ruby
# == Schema Information
#
# Table name: user
#
#  id   :integer          not null, primary key
#  name :string           not null


# Table name: score
#
#  id   :integer           not null, primary key
#  number :integer         not null
#  user_id :integer        not null
#  game_id :integer        not null


# Table name: game
#
#  id   :integer           not null, primary key
#  name :string            not null
#  game_maker_id :integer  not null
```

class Score

  belongs_to :user,
  primary_key: :id,
  foreign_key: :user_id,
  class_name: :User

  belongs_to :game,
  primary_key: :id,
  foreign_key: :game_id,
  class_name: :Game

end

class User

  has_many :scores,
  primary_key: :id,
  foreign_key: :user_id
  class_name: :Score

  has_many :game_makers,
  primary_key: :id,
  foreign_key: :game_maker_id,
  class_name: :Game


end

class Game

  has_many :scores,
  primary_key: :id,
  foreign_key: :game_id,
  class_name: :Score

  belongs_to :game_maker,
  primary_key: :id,
  foreign_key: :game_maker_id
  class_name: User

end



#### Solution

```ruby
class Score < ApplicationRecord
  belongs_to :user,
    class_name: 'User',
    foreign_key: :user_id,
    primary_key: :id

  belongs_to :game,
    class_name: 'Game',
    foreign_key: :game_id,
    primary_key: :id
end

class User < ApplicationRecord
  has_many :games_made,
    class_name: 'Game',
    foreign_key: :game_maker_id,
    primary_key: :id,
    optional: true

  has_many :scores,
    class_name: 'Score',
    foreign_key: :user_id,
    primary_key: :id

end

class Game < ApplicationRecord
  belongs_to :game_maker,
    class_name: 'User',
    foreign_key: :game_maker_id,
    primary_key: :id

  has_many :scores,
    class_name: 'Score',
    foreign_key: :game_id,
    primary_key: :id
end

```
