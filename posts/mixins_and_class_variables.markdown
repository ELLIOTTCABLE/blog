Mixins and Class Variables
==========================

I've had a recurring Ruby problem that I've had to re-solve every time I come
across it - so I'll document it here once and for all. Usually, I'll be
writing a module to be mixed in to a class - aka a mixin - that needs to
provide some sort of function to the class that needs a persistent store
unique to that particular class. Just using a class variable isn't enough,
because that class variable will be global to all classes that mixin the
module (since the class variable will be a variable on the module object, not
on the class objects).

It turns out that the solution is to metaprogrammatically get and set the
class variable whenever necessary, using `Module#class_variable_get` and
`Module#class_variable_set` instead of simply calling the variable. When this
happens, it's done on `klass`, not on the module.

    class Foo
      def self.inherited klass
        klass.send :class_variable_set, :@@attribute_initializations, []

        klass.class_eval do
          def self.bar
            self.send :class_variable_get, :@@attribute_initializations
          end
        end

      end
    end

This method, though ugly, is fairly robust. Try playing with the following -
we now have a seperate array for each class that subclasses Foo.

    class A < Foo
      p @@attribute_initializations
      @@attribute_initializations << :a
      p @@attribute_initializations
    end

    class B < Foo
      p @@attribute_initializations
      @@attribute_initializations << :b
      p @@attribute_initializations
    end
    
    p A.bar
    p B.bar

This works for our scenario as well - a module acting as a mixin:

    module Bar
      def self.included klass
        klass.send :class_variable_set, :@@attribute_initializations, []

        klass.class_eval do
          def self.bar
            self.send :class_variable_get, :@@attribute_initializations
          end
        end

      end
    end
    
    class A
      include Bar

      p @@attribute_initializations
      @@attribute_initializations << :a
      p @@attribute_initializations
    end

    class B
      include Bar

      p @@attribute_initializations
      @@attribute_initializations << :b
      p @@attribute_initializations
    end

    p A.bar
    p B.bar