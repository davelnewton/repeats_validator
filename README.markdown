# Repeats Validator

Simplistic validator that handles arbitrary "repeat up to __n__ times" validations.

The counting mechanism is passed in to the validator via the options hash.

In addition, the validator will optionally add methods to the class under validation to indicate a repeats-validation-specific error, avoiding having to check the model's errors collection for both the field and error type.

For example, assume a requirement that entries in a sweepstakes only allow three entries per email address.

    class Entry
        # ...
        validates :email, :presence => true,
                          :email => true,
                          :repeats => {
	                          :upto => 3,
	                          :add_methods => true,
	                          :count_with => lambda { |rec, attr, val| Entry.count(TODO),
                          }
    end

Then in a controller, say, we can check for this error specifically, and cleanly.

    def create_entry
      # ...
      if @entry.save
          # ...
      else
          render @entry.email_repeats_error ? 'too_many_entries' : 'new'
      end
    end
