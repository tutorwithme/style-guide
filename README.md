# Tutor With Me Style Guide

This is the official guide for developers at Tutor With Me.

Note that while certain rules may not apply in all cases, it's often safer to follow the rules outlined here.

## Ruby

### Margin

Keep lines fewer than 80 characters long. Enable the margin line option in your IDE if you have this function available.

### Indention

* Use soft-tabs (spaces instead of `\t` characters) with a two space indent.  
  This might be a setting in your IDE; `TAB ->` key presses should create two space indentions automatically.

* Break long chained method calls into multiple lines. Each line should contain only one method call and be aligned to the first `.` on the first line.

        @record = Record.find_by_user_id(user_id)
                        .order('name DESC')
                        
* Indent the `public`, `protected`, and `private` methods as much the method definitions they apply to. Leave one blank line above them.

        class SomeClass
          def public_method
            # ...
          end
        
          private
          def private_method
            # ...
          end
        end

### White Space

* Add a single space after an inline comment (`#`)

        def some_method
          # does something
        end


* No spaces after `(`, `[` or before `]`, `)`.
    
        some(arg).other
        [1, 2, 3].length
        
* Use spaces around operators, after commas, colons and semicolons, around { and before }.

        sum = 1 + 2
        a, b = 1, 2
        1 > 2 ? true : false; puts 'Hi'
        [1, 2, 3].each { |e| puts e }

    
* Do use parentheses `(` `)` around parameters in method definitions.

         def some_method
           # body omitted
         end
        
         def some_method_with_arguments(arg1, arg2)
           # body omitted
         end
         
* Do use parentheses `(` `)` around parameters when making method calls on an object.

         @user = User.find(params[:user_id])
         
* Do not use parentheses `(` `)` around `if` conditions unless the condition contains an assignment.
        
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

* Do not use parentheses `(` `)` in views when using Rails helper methods in views.

        link_to 'Help', help_path
        
* Never put a space between a method name and the opening parenthesis.
        
        # bad
        f (3 + 2) + 1
        
        # good
        f(3 + 2) + 1
                    
* Use `\` when a method requires a long list of parameters. Align the parameters below the beginning of the method chain (if any) and indent again.

        alert = Alert.new \
                  user_id: user_id
                  resolved: false
                  message: "This is a new alert."
          
* Use empty lines between `def`s and to break up a method into logical paragraphs.

        def some_method
          data = initialize(options)
        
          data.manipulate!
        
          data.result
        end
        
        def some_method
          result
        end
        
* Use spaces around the `=` operator when assigning default values to method parameters.

        # bad
        def some_method(arg1=:default, arg2=nil, arg3=[])
          # do something...
        end
        
        # good
        def some_method(arg1 = :default, arg2 = nil, arg3 = [])
          # do something...
        end

### Syntax

* Avoid return where not required.

        # bad
        def some_method(some_arr)
          return some_arr.size
        end
        
        # good
        def some_method(some_arr)
          some_arr.size
        end

* Use `and` or `or` in conditional statements

        if this and that
            # true
        end

* Do not use a negative `if` condition with an `else` block. Use a positive case first.

        # bad
        if not user.active?
          user.status = 'inactive'
        else
          user.status = 'active'
        end
        
        # not so bad: using unless
        unless user.active?
          user.status = 'inactive'
        else
          user.status = 'active'
        end
        
        # better: try inverting the code blocks
        if user.active?
          user.status = 'active'
        else
          user.status = 'inactive'
        end
        
        # best: try using a positive conditional
        if user.inactive?
          user.status = 'inactive'
        else
          user.status = 'active'
        end
        
* Avoid using the ternary operator (`?:`) except in cases when the expression is trivial or in views.

* Favour modifier `if/unless` usage when you have a single-line body.
        
        # bad
        if some_condition
          do_something
        end
        
        # good
        do_something if some_condition
        
* Never use `for`, unless you know exactly why. Most of the time iterators should be used instead. `for` is implemented in terms of `each` (so you're adding a level of indirection), but with a twist - `for` doesn't introduce a new scope (unlike `each`) and variables defined in its block will be visible outside it.

        arr = [1, 2, 3]
        
        # bad
        for elem in arr do
          puts elem
        end
        
        # good
        arr.each { |elem| puts elem }
        
* Never use `then` for multi-line `if`/`unless`.

        # bad
        if some_condition then
          # body omitted
        end
        
        # good
        if some_condition
          # body omitted
        end
        
* Use one expression per branch in a ternary operator (`?:`). This also means that ternary operators must not be nested. Prefer `if`/`else` constructs in these cases.

        # bad
        some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else
        
        # good
        if some_condition
          nested_condition ? nested_something : nested_something_else
        else
          something_else
        end
        
* Rescue blocks don't need to be tied to a 'begin'

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
        
* Use _ for unused block parameters.
    
        # bad
        result = hash.map { |k, v| v + 1 }
        
        # good
        result = hash.map { |_, v| v + 1 }
        
#### Idioms

* Use `||=` freely to initialize variables.

        # set name to Bob, only if it is nil or false
        name ||= 'Bob'
        
        # equivalent
        (name || (name = ('Bob')))
        
        # not equivalent but similar
        if name == nil || name == false
          name = 'Bob'
        end
        
* Don't use `||=` to initialize boolean variables. (Consider what would happen if the current value happened to be `false`.)
        
        # bad - would set enabled to true even if it was false
        enabled ||= true
        
        # good
        enabled = true if enabled.nil?
        
* Use `%w()` to initialize arrays for hard coded values

        # bad
        STATUS = ['SUCCESS', 'FAILED']

        STATUS = %w(SUCCESS FAILED)
        
        # => ["SUCCESS", "FAILED"]

* Quick mass assignment

        a, b, c, d = 1, 2, 3, 4
        
* Array destructuring

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


* Use ranges instead of complex comparisons for numbers

        year = 1972
        puts  case year
                when 1970..1979: "Seventies"
                when 1980..1989: "Eighties"
                when 1990..1999: "Nineties"                                
              end
              
* Use labeled parameters to improve readability

        def link_to(body, url, html_options = {})
          # body omitted
        end
        
        link_to 'Help', '/help', class: 'bold', target: '_blank'
        # => <a href="/help" class="bold" target="_blank">Help</a>
        
        def arguments_and_opts(*args, opts)
          puts "arguments: #{args} options: #{opts}"
        end
        
        arguments_and_opts 1,2,3, :a=>5
        # arguments: [1, 2, 3] options: {:a=>5}
        
* Use the splat operator for a variable number of arguments

        def say(what, *people)
          people.each{|person| puts "#{person}: #{what}"}
        end
        
        say "Hello!", "Alice", "Bob", "Carl"
        # => Alice: Hello!
        # => Bob: Hello!
        # => Carl: Hello!
        
### Strings
-----------

* Prefer string interpolation to concatenation

        # bad
        message = "Successfully deleted " + name + "."

        # good
        message = "Successfully deleted #{name}."

* Use single quote strings unless you need string interpolation

        # ok
        message = "Success"
        
        # better
        message = 'Success'
        
        # good
        message = "Successfully deleted #{name}."
        
* Use `<<` operator to concatonate strings instead of `+`, when inside loops. ([More on this.](http://blog.purepistos.net/index.php/2008/07/14/benchmarking-ruby-string-interpolation-concatenation-and-appending/))

        message = ""
        
        # ok
        users.each do |user|
          message += user.full_name
        end
        
        # better
        users.each do |user|
          message <<= user.full_name
        end


### Naming
----------
* Use `snake_case` for methods and variables.
* Use `CamelCase` for classes and modules. (Keep acronyms like `HTTP`, `RFC`, `XML` uppercase.)
* Use `SCREAMING_SNAKE_CASE` for other constants.
* The names of predicate methods (methods that return a boolean value) should end in a question mark. (i.e. `Array#empty?`). Do not prefix these with `is_`: the question mark already indicates that it is a predicate.
* The names of potentially "dangerous" methods (i.e. methods that modify `self` or the arguments, `exit!`, etc.) should end with an exclamation mark. Bang methods should only exist if a non-bang method exists. ([More on this](http://dablog.rubypal.com/2007/8/15/bang-methods-or-danger-will-rubyist)).

        
### Principles
--------------

#### DRY

Don't Repeat Yourself: as much as possible try to package your code in a way that limits the amount of duplication. This typically amounts to creating a method that is called by disparate parts of the application or to add common methods of similar objects into a parent class.

**When to use this technique:** When you have multiple lines of code that is identical.

#### Return early / return often

When reading code one must keep in mind all the variables and conditions to understand when a certain piece of code will be run. When faced with multiple cases that requires a lot of nested conditionals returning early is technique that helps reduce these number of variables that need to be memorized as cases are dismissed instead of stacked on each other. Consider this piece of code:

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
    
Because of the else condition we cannot simplify this into a single `if` condition. Let's use return instead:

    def send_alert(user)
      return unless user.active? or user.has_alerts?
      
      if user.notifications.receive_emails?
         user.notifications.send
      else
        user.messages.send
      end
    end
    

Here is an example where an implied `return` statement is used:

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
    
**When to use this technique:** When you have one or more levels of indention.

#### MVC and Fat Models, Skinny Controllers

MVC stands for Model-View-Controller. The flow typically goes from Controller to Model and then finally View. Let's look at the sort of code each file contains:

##### Models

Models deal with data and all the business logic. In Rails models typically are direct representations of a database table. Rails models contain (but not limited to) validations, attribute access protection, associations, callbacks and user methods.

##### Views

Views are the output that is displayed to the user. In web framework like Rails that means HTML code. More often than not it is combined with Erb (Embedded Ruby) for conditionals or calling helpers (i.e., creating links).

**PITFALL: Controller logic in views**

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
    
In the code above the "C" in MVC is completely bypassed and results in excessive Ruby code in a view and is violating the MVC pattern. The `people = Person.find(...)` statement belongs in the controller and `person` should be an instance variable (`@people`) so that it can be used in the view.

##### Controllers

Controllers are the entry point of each page the user requests and deal with the inputs. This is also where you determine the models you will retrieve from and the view you are going to display (this is done automatically, however).

So what does "Fat Models, Skinny Controllers" mean? It means to write most logic into models instead of controllers. This results in more code in models than controllers. In Rails, this is good because more often that not you end up having more controllers than models yet many controllers use the same models and helps reduce repetition (see DRY).

**When to use this technique:** Always. If you have more than 10 lines of controller code it's a sign you need to move some code into the model.

#### Decorators

Rails does not support decorators by default. We use the `draper` gem to decorate our models.

What are decorators? They are like helper methods wrapped around models (data) that deal with presentation. For example if you have a `price` field in a `Course` model, then displaying this value should have the currency next to it, be of exactly 2 decimal places and use a thousand separator when needed. This type of logic can be done in the model but a decorator is best suited to do this as it deals strictly with formatting.

How do decorators work? Decorators need a model to "decorate." A decorator cannot exist without an underlaying model. In programming terms, it's basically sub-classing a model class. Here's an example:

    class CourseDecorator < ApplicationDecorator
        decorates :course
        def price
          return t('messages.courses.free') if model.hourly_rate == 0
          h.number_to_currency(price_with_commission, precision: 2, unit: 'CAD$')
        end
    end
    
**When to use this technique:** When outputting an attribute of a model which needs special formatting and is used in more than one place.