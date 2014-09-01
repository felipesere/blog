Validation vs. Database OR Validation _with_ the Database

This post is purely hypothetical.
It came up last week as we were discussing were to place validations and handle constraints on the database.

Let's build a hypotheical application. 
In that application, the user gets to send data through a run-of-the-mill  HTML form.
Since your application is security and UX-concious, you do a form validation when the data arrives at your server.
Errors get nicely reported to the user and not propagated further down stream.

Assume now, that you you add a unique-constraint on your database on column called ``foo``.
How will you deal with that new constraint?


#3 The Problem

In Rails, you can use validations.
Among those validation is a so-called ``uniqueness`` validation.
What is does is perform a ``SELECT`` against your database and make sure that the record you are trying to enter will not collide against a pre-existing one.
That is a potential race-condition waiting to happen.
Only reading does from the databse does not guarantee that the next insertion will not hit a unique constraint violation.
Splitting the ``check`` phase (in this case the validation) from the actual operation means that there is a window of opportunity where  a seprate ``check`` will not see the soon-to-be-commited record.

The only way to get around is to do check-and-set in a single, atomic operation.
In this case, that means actually __writing__ to the database and checking for a violation of the unque constraint.
If no exception is raised, we know that value was/is correctly unique and we have reserved it.
The next call to form validation will raise then raise a UniqueConstraintViolation and deny that record.

#3 To be or not to be valid
Now, this brings up a few other issues.
For simplicity, lets assume we are still in a _Railsy_ application.
At least to the extent that we can call ``valid?`` on a form to check its validity.
If ``valid?`` is ``true`` we will carry on with the operation or otherwise extract and display the errors to the users.

``valid?`` is a query and it should not have side-effects like writing records to the DB.
Then again, calling ``valid?`` followed by a normal operation and then adding errors to the form once the exception flies is not really clean either.
It means "valid" is not quite "valid".

#3 An optimistic locking approach
One approach would be to try to get that ``check-and-set`` to be part of the validation.
To this end, we would create a table ``foos`` which has as a single column with all the values for ``foo``.
Let that new table have a relationship with the original table so that entering a value for the __real__ entity ensures that ``foo`` is present in the ``foos`` table.
When running the validation, the ``valid?`` method will insert the value into the ``foos`` table (or fail) and thus ensure the UniqueConstraint Violation is kept without polluting the original data.

The code using the validation would remain the same:

if @form.valid?
  run_create_operation(@form.attributes)
   ...
   else
   ...fail..
end

The ``valid?`` method would go ahead and use some kind of ``UniqueSlugValidator``:

def validate(record)
   insert_into_uniqueness_table(record.foo)
rescue UniqueConstraintViolationException => e
    record.errors[:foo] << "A record with foo #{record.foo} already exists"
end

The beauty here is that the code is small an coheisve.
Checking a unique constraint violation is part of the validation process.
This becomes apparent in that the code in did not have to change, the code in the validator is small and concise and finally, the error message gets naturally added, without having to delegate it across multiple boundaries or doing anything odd.


# Phase splitting
Alternatively add a second phase of "validity". The current "valid?" would mean 'this represents valid data from the browser", while the second phase would mean "this was valid in the database".
In my opinion, this makes the code read bit unclear:


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
   update_errors_in_form(create.errors)
   render_invalid_form
 end

 Though this decouples the controller and validation from the database, it still has its perils.
 There are now two places that update the errors in a form

   * ``valid?`` where most of the validations take place
   * ``update_errors_in_form`` where some specific error messages are added

That code will also have to be repeated across any method that inserts data that might produce a failure.

#3 Conclusion

I am not sure which variation is better, though I am leaning towards adding the table and the proper validation class.
It feels cleaner, more concise.
Plus, there is the added benefit of having global uniqueness across more than one table (in case ``foo`` is an attribute that exists in more than one entitz).
There is the question whether the _cost_ of carrying an extra table and binding one validation class to that table outweighs the _benefit_ of the cleaner code.

Are the better designs out there that I might have overlooked? 
Let me know in the comments section what you think! 
