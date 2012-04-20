Ruby Style Guide
================

Much of this was taken from [https://github.com/styleguide/ruby](https://github.com/styleguide/ruby) (which was consequently taken from [https://github.com/bbatsov/ruby-style-guide](https://github.com/bbatsov/ruby-style-guide)). Please add to this guide if you find any particular patterns or styles that we've adopted internally. Submit a pull request to ask for feedback (if you're an employee).

Margin
------

Keep lines fewer than 80 characters long. Enable the margin line option in your IDE if you have this function available.

Indention
---------

* Use soft-tabs (spaces instead of `\t` characters) with two spaces per indentation level. This might be a setting in your IDE; `TAB ->` key presses should create two space indentions automatically.

    ```ruby
    # good
    def some_method
      do_something
    end
    
    # bad - four spaces
    def some_method
        do_something
    end
    ```


* Align the parameters of a method call if they span over multiple lines.

    ```ruby
    # starting point (line is too long)
    def send_mail(source)
        Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end
    
    # bad (normal indent)
    def send_mail(source)
        Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end
    
    # bad (double indent)
    def send_mail(source)
        Mailer.deliver(
            to: 'bob@example.com',
            from: 'us@example.com',
            subject: 'Important message',
            body: source.text)
    end
    
    # good
    def send_mail(source)
        Mailer.deliver(to: 'bob@example.com',
                       from: 'us@example.com',
                       subject: 'Important message',
                       body: source.text)
    end
    
    # better (see white space rule below)
    def send_mail(source)
        Mailer.deliver \
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text
    end
    ```
                        
* Indent the `public`, `protected`, and `private` methods as much the method definitions they apply to. Leave one blank line above them.

    ```ruby
    class SomeClass
      def public_method
        # ...
      end

      private
      def private_method
        # ...
      end
    end
    ```

White Space
-----------

* No spaces after `(`, `[` or before `]`, `)`.

    ```ruby
    some(arg).other
    [1, 2, 3].length
    ``` 

* Use spaces around operators, after commas, colons and semicolons, around `{` and before `}`.

    ```ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```
    
* Use parentheses `(` `)` around parameters in method definitions when one or more parameters are needed.

    ```ruby
    def some_method
      # body omitted
    end
    
    def some_method_with_arguments(arg1, arg2)
      # body omitted
    end
    ```
   
* Omit parentheses around parameters for methods that are part of an internal DSL (e.g. Rake, Rails, RSpec), methods that are with "keyword" status in Ruby (e.g. attr_reader, puts) and attribute access methods. Use parentheses around the arguments of all other method invocations.

    ```ruby
    class Person
      attr_reader :name, :age
    
      # omitted
    end
    
    temperance = Person.new('Temperance', 30)
    temperance.name
    
    puts temperance.age
    
    x = Math.sin(y)
    array.delete(e)
    
    link_to 'Help', help_path
    ```
    
* Omit parentheses around `if` conditions unless the condition contains an assignment.

    ```ruby
    # bad
    if (x > 10)
      # body omitted
    end

    # good
    if x > 10
      # body omitted
    end

    # ok
    if (x = self.next_value)
      # body omitted
    end
    ```
        
* Never put a space between a method name and the opening parenthesis.

    ```ruby
    # bad
    f (3 + 2) + 1

    # good
    f(3 + 2) + 1
    ```
                    
* Use `\` when a method requires a long list of parameters. Align the parameters below the beginning of the method chain and indent it.

    ```ruby
    alert = Alert.new \
              user_id: user_id
              resolved: false
              message: "This is a new alert."
    ```
          
* Use empty lines between `def`s and to break up a method into logical paragraphs.

    ```ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```
        
* Use spaces around the `=` operator when assigning default values to method parameters.

    ```ruby
    # bad
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # good
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

Syntax
------

* Use Ruby 1.9 literal hash syntax in preference to the the hashrocket syntax.

    ```ruby
    # bad
    Article.where(:published => true).all

    # good
    Article.where(published: true).all        
    ```
    
* Use the new lambda literal syntax.

    ```ruby
    # bad
    lambda = lambda { |a, b| a + b }
    lambda.call(1, 2)
    
    # good
    lambda = ->(a, b) { a + b }
    lambda.(1, 2)
    ```

* Avoid return when not required (remember methods (implicitly) always return something in Ruby)

    ```ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```

* Use `and` or `or` in conditional statements and control flow.

    ```ruby
    if this and that
        # true
    end
    
    return if require_admin or require_moderator
    ```

* Do not use a negative `if` condition with an `else` block. Use a positive case first.

    ```ruby
    # bad
    if not user.active?
      user.status = 'inactive'
    else
      user.status = 'active'
    end

    # passable (using unless)
    unless user.active?
      user.status = 'inactive'
    else
      user.status = 'active'
    end

    # better (inverting the body)
    if user.active?
      user.status = 'active'
    else
      user.status = 'inactive'
    end

    # best (positive case first)
    if user.inactive?
      user.status = 'inactive'
    else
      user.status = 'active'
    end
    ```
        
* Avoid using the ternary operator (`?:`) except in cases when the expression is trivial or in views.

* Favour modifier `if`/`unless` usage when you have a single-line body.
   
    ```ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition
    ```
        
* Never use `for`, unless you know exactly why. Most of the time iterators should be used instead. `for` is implemented in terms of `each` (so you're adding a level of indirection), but with a twist - `for` doesn't introduce a new scope (unlike `each`) and variables defined in its block will be visible outside it.

    ```ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # good
    arr.each { |elem| puts elem }
    ```

* Never use `then` for multi-line `if`/`unless`.

    ```ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* Use one expression per branch in a ternary operator (`?:`). This also means that ternary operators must not be nested. Prefer `if`/`else` constructs in these cases.

    ```ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```
        
* Rescue blocks don't need to be tied to a `begin`.

    ```ruby
    # extraneous begin block
    def x
      begin
        # ...
      rescue
        # ...
      end
    end

    # good
    def x
      # ...
    rescue
      # ...
    end
    ```

* Use `_` for unused block parameters.

    ```ruby    
    # bad
    result = hash.map { |k, v| v + 1 }

    # good
    result = hash.map { |_, v| v + 1 }
    ```

Idioms
------

* Use `||=` freely to initialize variables.

    ```ruby
    # set name to Bob, only if it is nil or false
    name ||= 'Bob'

    # equivalent
    (name || (name = ('Bob')))

    # not equivalent but similar
    if name == nil || name == false
      name = 'Bob'
    end
    ```

* Don't use `||=` to initialize boolean variables. (Consider what would happen if the current value happened to be `false`.)

    ```ruby
    # bad - would set enabled to true even if it was false
    enabled ||= true

    # good
    enabled = true if enabled.nil?
    ```

* Use `%w()` to initialize arrays for hard coded values.

    ```ruby
    # bad
    STATUS = ['SUCCESS', 'FAILED']

    STATUS = %w(SUCCESS FAILED)

    # => ["SUCCESS", "FAILED"]
    ````

* Quick mass assignment:

    ```ruby
    a, b, c, d = 1, 2, 3, 4
    ````

* Array destructuring:

    ```ruby
    first, *list = [1, 2, 3, 4]
    # first => 1
    # list => [2,3,4]

    *list, last = [1, 2, 3, 4]
    # list => [1, 2, 3]
    # last => 4

    first, *mid, last = [1, 2, 3, 4]
    # first => 1
    # mid => [2, 3]
    # last => 4
    ```

* Use ranges instead of complex comparisons for numbers.

    ```ruby
    year = 1972
    puts  case year
          when 1970..1979: "Seventies"
          when 1980..1989: "Eighties"
          when 1990..1999: "Nineties"
          end
    ```

* Use labeled parameters to improve readability.

    ```ruby
    def link_to(body, url, html_options = {})
      # body omitted
    end

    link_to 'Help', '/help', class: 'bold', target: '_blank'
    # => <a href="/help" class="bold" target="_blank">Help</a>

    def arguments_and_opts(*args, opts)
      puts "arguments: #{args} options: #{opts}"
    end

    arguments_and_opts 1, 2, 3, a: 5
    # => arguments: [1, 2, 3] options: { :a => 5 }
    ```
    
* Use the splat operator (`*`) for a variable number of arguments.

    ```ruby
    def say(what, *people)
      people.each{|person| puts "#{person}: #{what}"}
    end

    say "Hello!", "Alice", "Bob", "Carl"
    # => Alice: Hello!
    # => Bob: Hello!
    # => Carl: Hello!
    ```

Strings
-------

* Prefer string interpolation to concatenation.

    ```ruby
    # bad
    message = "Successfully deleted " + name + "."

    # good
    message = "Successfully deleted #{name}."
    ```

* Use single quote strings unless you need string interpolation.

    ```ruby
    # ok
    message = "Success"

    # better
    message = 'Success'

    # good
    message = "Successfully deleted #{name}."
    ```
        
* Avoid using `String#+` when you need to construct large data chunks. Instead, use `String#<<`. Concatenation mutates the string instance in-place and is always faster than `String#+`, which creates a bunch of new string objects.
    
    ```ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'
    
    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```
* Use `%()` for single-line strings which require both interpolation and embedded double-quotes. For multi-line strings, prefer heredocs.

    ```ruby
    # bad (no interpolation needed)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>'
    
    # bad (no double-quotes)
    %(This is #{quality} style)
    # should be "This is #{quality} style"
    
    # bad (multiple lines)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.
    
    # good (requires interpolation, has quotes, single line)
    %(<tr><td class="name">#{name}</td>)
    ```
    
Naming
------

* Use `snake_case` for methods and variables.
* Use `CamelCase` for classes and modules. (Keep acronyms like `HTTP`, `RFC`, `XML` uppercase.)
* Use `SCREAMING_SNAKE_CASE` for other constants.
* The names of predicate methods (methods that return a boolean value) should end in a question mark. (i.e. `Array#empty?`). Do not prefix these with `is_`: the question mark already indicates that it is a predicate.
* The names of potentially "dangerous" methods (i.e. methods that modify `self` or the arguments, `exit!`, etc.) should end with an exclamation mark. Bang methods should only exist if a non-bang method exists. ([More on this](http://dablog.rubypal.com/2007/8/15/bang-methods-or-danger-will-rubyist)).

Comments
--------

* Write self-documenting code and ignore the rest of this section. Seriously!
* Use one space after `#`.
* Comments longer than a word are capitalized and use punctuation. Use one space after periods.
* Avoid superfluous comments.

    ```ruby
    # bad
    counter += 1 # increments counter by one
    ```

* Keep existing comments up-to-date. An outdated is worse than no comment at all.
Avoid writing comments to explain bad code. Refactor the code to make it self-explanatory. (Do or do not - there is no try.)

Annotations
-----------

Annotations should usually be written on the line immediately above the relevant code.
The annotation keyword is followed by a colon and a space, then a note describing the problem.
If multiple lines are required to describe the problem, subsequent lines should be indented two spaces after the #.

```ruby
def bar
    # FIXME: This has crashed occasionally since v3.2.1. It may
    #   be related to the BarBazUtil upgrade.
    baz(:quux)
end
```

In cases where the problem is so obvious that any documentation would be redundant, annotations may be left at the end of the offending line with no note. This usage should be the exception and not the rule.

```ruby
def bar
  sleep 100 # OPTIMIZE
end
```

* Use `TODO` to note missing features or functionality that should be added at a later date.
* Use `FIXME` to note broken code that needs to be fixed.
* Use `OPTIMIZE` to note slow or inefficient code that may cause performance problems.
* Use `HACK` to note code smells where questionable coding practices were used and should be refactored away.
* Use `REVIEW` to note anything that should be looked at to confirm it is working as intended. For example: `REVIEW: Are we sure this is how the client does X currently?`
* Use other custom annotation keywords if it feels appropriate, but be sure to document them in your project's `README` or similar.

Principles
----------

### DRY

Don't Repeat Yourself: as much as possible try to write your code in a way that limits the amount of duplication. This typically amounts to creating a method in a helper, library or parent class so that it can be accessed where needed instead of being duplicated.

**When to use this technique:** When you have multiple lines of code that are identical and needed in more than one place in the application.

### Return early / return often

When reading code one must keep in mind all the variables and conditions to understand when a certain piece of code will be run. When faced with multiple cases that requires a lot of nested conditionals returning early is technique that helps reduce these number of variables that need to be memorized as cases are dismissed instead of stacked on each other. Consider this piece of code:

```ruby
def send_alert(user)
  if user.active?
    if user.has_alerts?
      if user.notifications.receive_emails?
        user.notifications.send
      else
    	user.messages.send
      end
    end
  end
end
```
Because of the else condition we cannot simplify this into a single `if` condition. Let's use return instead:

```ruby
def send_alert(user)
  return unless user.active? or user.has_alerts?
  
  if user.notifications.receive_emails?
     user.notifications.send
  else
    user.messages.send
  end
end
```

Here is an example where an implied `return` statement is used:

```ruby
# can be refactored
def delete
  if valid?
    delete!
  end
end

# refactored
def delete
  delete! if valid?
end
```    
**When to use this technique:** When you have one or more levels of indention.

### MVC and Fat Models, Skinny Controllers

MVC stands for Model-View-Controller. The flow typically goes from Controller to Model and then finally View. Let's look at the sort of code each file contains:

#### Models

Models deal with data and all the business logic. In Rails, models typically are direct representations of a database table. Rails models contain (but not limited to) validations, attribute access protection, associations, callbacks and user methods.

#### Views

Views are the output that is displayed to the user. In web framework like Rails that means HTML code. More often than not it is combined with Erb (Embedded Ruby) for conditionals or calling helpers (i.e., creating links).

**PITFALL: Controller logic in views**

```rhtml
<!-- app/views/people/index.rhtml -->
<% people = Person.find(
      :conditions => ["added_at > ? and deleted = ?", Time.now.utc, false],
      :order => "last_name, first_name") %>
<% people.reject { |p| p.address.nil? }.each do |person| %>
  <div id="person-<%= person.new_record? ? "new" : person.id %>">
    <span class="name">
      <%= person.last_name %>, <%= person.first_name %>
    </span>
    <span class="age">
      <%= (Date.today - person.birthdate) / 365 %>
    </span>
  </div>
<% end %>
```
    
In the code above the "C" in MVC is completely bypassed and results in excessive Ruby code in a view and is violating the MVC pattern. The `people = Person.find(...)` statement belongs in the controller and `person` should be an instance variable (`@people`) so that it can be used in the view.

#### Controllers

Controllers are the entry point of each page the user requests and deal with the inputs. This is also where you determine the models you will retrieve from and the view you are going to display (this is done automatically, however).

So what does "Fat Models, Skinny Controllers" mean? It means to write most logic into models instead of controllers. This results in more code in models than controllers. In Rails, this is good because more often that not you end up having more controllers than models yet many controllers use the same models and helps reduce repetition (see DRY).

**When to use this technique:** Always. If you have more than 10 lines of controller code it's a sign you need to move some code into the model.

### Decorators

Rails does not support decorators by default. We use the `draper` gem to decorate our models.

What are decorators? They are like helper methods wrapped around models (data) that deal with presentation. For example if you have a `price` field in a `Course` model, then displaying this value should have the currency next to it, be of exactly 2 decimal places and use a thousand separator when needed. This type of logic can be done in the model but a decorator is best suited to do this as it deals strictly with formatting.

How do decorators work? Decorators need a model to "decorate." A decorator cannot exist without an underlaying model. In programming terms, it's basically sub-classing a model class. Here's an example:

```ruby
class CourseDecorator < ApplicationDecorator
  decorates :course
  def price
    return t('messages.courses.free') if model.hourly_rate == 0
    h.number_to_currency(price_with_commission, precision: 2, unit: 'CAD$')
  end
end
```
    
**When to use this technique:** When outputting an attribute of a model which needs special formatting and is used in more than one place.