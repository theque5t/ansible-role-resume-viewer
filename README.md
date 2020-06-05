Role Name
=========

Ansible role for resume viewing.


[![asciicast](https://asciinema.org/a/lDrThsLjvtQvfiQ4VuJDzLaKl.svg)](https://asciinema.org/a/lDrThsLjvtQvfiQ4VuJDzLaKl?autoplay=true&loop=true)

Requirements
------------

None.

Role Variables
--------------

Variable specifications are listed below (see also `default/main.yml`):

- `resume_view_resume`
    - description: The structured resume data to be viewed
    - aliases:
        - `resume`
    - required: `true`
    - type: `dict`
    - defaults: 
```python
{
  'contact': {
    'first_name': '<first_name>',
    'middle_name': '<middle_name>',
    'last_name': '<last_name>',
    'phone': '<phone>',
    'email': '<email>',
    'linkedin': '<linkedin>',
    'github': '<github>'
  },
  'summary': '<This is an example summary sentence. This is an example summary sentence. This is an example summary sentence. This is an example summary sentence.>',
  'experience': [
    {
      'title': '<title>',
      'company': '<company>',
      'location': '<location>',
      'start_date': '<start_date>',
      'end_date': '<end_date>',
      'description':
        [
          'This is an example description sentence.',
          'This is an example description sentence. This is an example description sentence. This is an example description sentence.',
          'This is an example description sentence. This is an example description sentence.'
        ]
    },
    {
      'title': '<title>',
      'company': '<company>',
      'location': '<location>',
      'start_date': '<start_date>',
      'end_date': '<end_date>',
      'description':
        [
          'This is an example description sentence.',
          'This is an example description sentence. This is an example description sentence. This is an example description sentence.',
          'This is an example description sentence. This is an example description sentence.'
        ]
    },
    {
      'title': '<title>',
      'company': '<company>',
      'location': '<location>',
      'start_date': '<start_date>',
      'end_date': '<end_date>',
      'description':
        [
          'This is an example description sentence.',
          'This is an example description sentence. This is an example description sentence. This is an example description sentence.',
          'This is an example description sentence. This is an example description sentence.'
        ]
    }
  ]
}
```
- `resume_viewer_title_header_template`
    - description: The template used when rendering the title header.
    - aliases: None.
    - required: `false`
    - type: `str`
    - defaults: 
```jinja
  {{ [ resume_viewer_resume.contact.first_name,
       resume_viewer_resume.contact.middle_name,
       resume_viewer_resume.contact.last_name] | 
       select('string') |
       join(' ') }}
```

- `resume_viewer_title_body_template`
    - description: The template used when rendering the title body.
    - aliases: None.
    - required: `false`
    - type: `str`
    - defaults: 
```jinja
"{{ omit }}"
```

- `resume_viewer_summary_header_template`
    - description: The template used when rendering the summary header.
    - aliases: None.
    - required: `false`
    - type: `str`
    - defaults: 
```jinja
Summary
```

- `resume_viewer_summary_body_template`
    - description: The template used when rendering the summary body.
    - aliases: None.
    - required: `false`
    - type: `str`
    - defaults: 
```jinja

  {{  resume_viewer_resume.summary | wordwrap(60) | indent(4, first=True) }}

```

- `resume_viewer_experience_header_template`
    - description: The template used when rendering the experience header.
    - aliases: None.
    - required: `false`
    - type: `str`
    - defaults: 
```jinja
Experience
```

- `resume_viewer_experience_body_template`
    - description: The template used when rendering the experience body. `Note`: The experience list is iterated through. This template is rendered for each item. Reference the item using the value `job`.
    - aliases: None.
    - required: `false`
    - type: `str`
    - defaults: 
```jinja

  {{ (job.title+' @ '+job.company) | indent(4, first=True) }}
  {{ (job.start_date+' - '+ job.end_date) | indent(4, first=True) }}
  {{ job.location | indent(4, first=True) }}
  {{ '------------------------------------------------------------------------' | indent(4, first=True) }}
  
  {% for item in job.description %}
      - {{ item | wordwrap(60) | indent(6) }}

  {% endfor %}

```

- `resume_viewer_contact_header_template`
    - description: The template used when rendering the contact header.
    - aliases: None.
    - required: `false`
    - type: `str`
    - defaults: 
```jinja
Contact Information
```

- `resume_viewer_contact_body_template`
    - description: The template used when rendering the contact body.
    - aliases: None.
    - required: `false`
    - type: `str`
    - defaults: 
```jinja

  {% for key, value in resume_viewer_resume.contact.items() %}
  {% if key in ['mobile', 'email', 'linkedin', 'github'] %}
  {{ "- %-10s: %s" | format((key | capitalize), value) | wordwrap(60) | indent(4, first=True) }}
  {% endif %}
  {% endfor %}

```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: localhost
  gather_facts: false
  roles:
    - theque5t.resume_viewer
```

Output:
```sh

PLAY [localhost] ***************************************************************

TASK [theque5t.resume_viewer : <first_name> <middle_name> <last_name>] *********
[theque5t.resume_viewer : <first_name> <middle_name> <last_name>]
Press enter to continue, Ctrl+C to interrupt:
ok: [localhost]

TASK [theque5t.resume_viewer : Summary] ****************************************
[theque5t.resume_viewer : Summary]

    <This is an example summary sentence. This is an example
    summary sentence. This is an example summary sentence. This
    is an example summary sentence.>
:
ok: [localhost]

TASK [theque5t.resume_viewer : Experience] *************************************
[theque5t.resume_viewer : Experience]

    <title> @ <company>
    <start_date> - <end_date>
    <location>
    ------------------------------------------------------------------------

    - This is an example description sentence.

    - This is an example description sentence. This is an example
      description sentence. This is an example description
      sentence.

    - This is an example description sentence. This is an example
      description sentence.

:
^Mok: [localhost] => (item=)
[theque5t.resume_viewer : Experience]

    <title> @ <company>
    <start_date> - <end_date>
    <location>
    ------------------------------------------------------------------------

    - This is an example description sentence.

    - This is an example description sentence. This is an example
      description sentence. This is an example description
      sentence.

    - This is an example description sentence. This is an example
      description sentence.

:
^Mok: [localhost] => (item=)
[theque5t.resume_viewer : Experience]

    <title> @ <company>
    <start_date> - <end_date>
    <location>
    ------------------------------------------------------------------------

    - This is an example description sentence.

    - This is an example description sentence. This is an example
      description sentence. This is an example description
      sentence.

    - This is an example description sentence. This is an example
      description sentence.

:
ok: [localhost] => (item=)

TASK [theque5t.resume_viewer : Contact Information] ****************************
[theque5t.resume_viewer : Contact Information]

    - Linkedin  : <linkedin>
    - Github    : <github>
    - Email     : <email>
:
ok: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

License
-------

MIT

Author Information
------------------

This role was created in 2020 by [Trevor Highfill](https://www.linkedin.com/in/trevor-highfill/)([@theque5t](https://github.com/theque5t)).
