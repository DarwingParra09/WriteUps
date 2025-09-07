This machine has a vulnerability for SSTI (Server-side Template Injection). 

![[01.Spookifier.png]]

The routes.py file is part of a Flask web application. It allows the user to enter text and return a modified message.

![[02.Spookifier.png]]
![[03.Spookifier.png]]
![[04.Spookifier.png]]
![[05.Spookifier.png]]

## How is SSTI detected?

Could send payloads in the parameters being rendered. 

- `${7*7}` (Mako, Velocity, etc.)
    
- `{{7*7}}` (Jinja2)
    
- `<%= 7*7 %>` (ERB - Ruby)
    
- `#{7*7}` (Twig)

![[06.Spookifier.png]]

The template is Mako, import the OS and I list the files and look for the flag.

![[07.Spookifier.png]]
![[08.Spookifier.png]]
