---
layout: post
title: Dynamic User Input with XML in C#
categories: dev code C# XML
tags:
- XML
- C#
- dev
...

When I started coding in medical software, one of my first major projects was to
create an online patient intake form for doctor's offices. While this may sound
simple, for someone who had basically no experience with server-client
architecture, it was actually the equivalent of throwing a new swimmer into the
diving well and letting them learn how to swim. Let me explain.  


Patient Intake refers to all those forms you fill out the first time you go to a
doctor's office. We wanted to eliminate this paperwork and also the additional
wait between arriving at the office and being seen by a physician, so the goal
was to make this form available for completion online prior to the appointment.
While creating a user input form and storing the data in an SQL server to push
to the doctor's client software later is obviously simple, the problem arose
when we discovered just *how* incredibly varied doctors' intake forms could be.
They were essentially a big mishmash of field types:  
  
* small text input (ex: patient name, medical complaint)  
* checkboxes (ex: family history of illnesses from list)  
* radio buttons (ex: are you currently a smoker?)  
* date boxes (ex: date of birth)  
  
To make things more complicated, a question which was vital for a podiatrist
might be completely useless for, say, an oncologist. Some doctors we surveyed
even had off-the-wall questions specific to their practice that we'd never seen
or considered before.  
  
Taking all these factors into account, we knew we'd have to make the Patient
Intake segment completely customizable within a set of different input types,
and the customization interface would need to be easy enough to alter so that
either end-users could write them up themselves, or alternatively, so that we
could create a simple user interface to do it for them. After examining some of
the different options we chose to go with a XML base. The reasons for this were
as follows:  
  
* we work with coders in several different physical locations and XML has
strong standards  
* we already had some frameworks in place for transmitting XML, so it
synergized well  
* some of our coders speak languages other than English and found XML simpler
than JSON, etc.  
* XML work well with MSSQL Server and can be queried directly
