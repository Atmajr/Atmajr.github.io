### Helping patients remember their medications

I've had a concept for a while now for a mobile or mobile-accessible web app to help patients keep track of the medications they take. In addition to a host of other features I'd like to build in eventually, even just a list of the pertinent details of the meds their on would be useful for patients when meeting a new doctor, visiting a new hospital or filling out medical paperwork. 

When tasked to build a CRUD application with account services for my Sinatra final project, I opted to build an alpha version of this application, with an eye toward expanding in the future.

### Technology

Most features required for this app were things I had done before, so there wasn't much to worry about. I used ActiveRecord to handle the SQLite DB for the backend of the site, having two models: 

- User, which handles user accounts including name, password, and e-mail addresses and have many medications.
- Medications, which comprise all details of each of a user's meds, and belong to a user.

Sinatra and [rack-flash](https://github.com/nakajima/rack-flash) were used to handle routing and error messages, respectively. In the future, I will not use rack-flash again and instead will be using sinatra-flash. I had to use a few work-arounds to make flash messages persist through redirects, where I could have used flash.keep if I'd used sinatra-flash instead. 

I thought about writing my own regex to control for validating e-mail address formatting, but in the end I instead went with the [email-validator](https://github.com/balexand/email_validator) gem. You can easily determine e-mail validity by passing the address via `EmailValidator.valid?` which makes things much simpler.

### Conclusion and next steps

In general, this was a fairly straight-forward project. The model interactions were one-way and simple, and because medications are private matters, there was no social aspect or inter-user interaction to worry about. 

In the end, the majority of concerns involved validating and authenticating. Validating that use input was good and valid, and authenticating that the user had the right to create and edit the information with which they were interacting.

In the future, I would like to take this app further in terms of both basic usability and feautres. In the short term, I'd like to look to some better responsive features, like info boxes than can collapse or expand to make viewing medication details easier instead of necessitating a page transition. Long-term, I'd like to roll this functionality into a mobile app which will allow push notifications for when to take medicine and possibly interact with the NHS API to list contraindications as well.

