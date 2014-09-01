validation vs database OR validation _with_ the databse

A form receives input from the user.
Sends that input to the server
First thing: check form.valid?
   @form.valid? will check conditions which are placed on the underlying model.
   those contraints are Rails constraints

Assume now, that you you have a unique-constraint on your database.

In this case, what those validations will do is do a select on the column with the constraint and check that the new value is unique. If it is unique, great, otherwiese add an error to the form and return it back.

that is a potential race condition waiting to happen.
Is that solvable?
only reading does not guarantee that the next write will hit a unique constraint violation.
The only way to get around is to do check-and-set in a single, atomic operation.
In this case, that means actually WRITING to the database and checking for a UniqueConstraintViolation.
If no exception is raised, we know that value was/is correctly unique and we have reserved it.
The next call to valid? will raise a UniqueConstraintViolation and mean that the data is invalid.
===> Add an image explaining this

Now, this brings up a few other issues.
valid? is a query and it should not have side-effects like writing stuff to the DB
then again, doing valid? and then adding errors to the form once the exception flies is not really clean either.
It means "valid" is not quite "valid".

Maybe there a couple of solutions to make this work:
Create a individual table :slugs that has as a single column with all the values, and let that new table have a relationship so that entering a value for the REAL entity ensures the :slug is present in the :slugs table.
When running the validation, the valid? method would insert the value into the :slugs table (or fail) and thus ensure the UniqueConstraint Violation is kept without polluting the original data.

The code using the validation would remain the same:

if @form.valid?
  run_create_operation(@form.attributes)
   ...
   else
   ...fail..
end

The ``valid?`` method would go ahead and use some kind of ``UniqueSlugValidator << ActiveModel::Validator``

def validate(record)
   insert_into_uniqueness_table(record.slug)
rescue UniqueConstraintViolationException => e
    record.errors[:slug] << "A page with slug #{record.slug} already exists"
end

The beauty here is that the code is small an coheisve. Checking a unique constraint violation is part of the validation process. This becomes apparent in that the code in (1) did not have to change, the code in the Validator is small and concise and finally, the error message gets naturally added, without having to delegate it across multiple boundaries or doing anything odd.


Alternatively add a second phase of "validity". The current "valid?" would mean 'this represents valid data from the browser", while the second phase would mean "this was valid in the databse".
That would make a bit of code in the middle awkward and a paranoid double check:


if @form.valid?
    create = operation_executor(@form.attributes)
    create.do!
    if create.succeeded?
       # ...return something meaningful...
    else
      update_errors_in_form(create.errors)
      render_invalid_form
    end
 else
   render_invalid_form
 end
