---
# Leave the homepage title empty to use the site title
title:
date: 2022-10-24
type: landing

sections:
  - block: about.avatar
    id: about
    content:
      username: admin
      text:
  - block: experience
    content:
      title: Experience
      date_format: Jan 2006
      items:
        - title: Research Fellow
          company: The University of Auckland
          company_url: ''
          company_logo: ''
          location: New Zealand
          date_start: '2022-02-01'
          date_end: ''
          description: Research fellow working in LISA data analysis.
        - title: PhD Student
          company: Monash University
          company_url: ''
          company_logo: 
          location: California
          date_start: '2016-01-01'
          date_end: '2020-12-31'
          description: Did some stuff 
      design:
      columns: '2'
  - block: collection
    content:
      title: Recent Publications
      text: |-
        {{% callout note %}}
        Quickly discover relevant content by [filtering publications](./publication/).
        {{% /callout %}}
      filters:
        folders:
          - publication
        exclude_featured: true
    design:
      columns: '2'
      view: citation
  - block: portfolio
    id: projects
    content:
      title: Projects
      filters:
        folders:
          - project
      default_button_index: 0
      buttons:
        - name: All
          tag: '*'
        - name: Deep Learning
          tag: Deep Learning
        - name: Other
          tag: Demo
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '1'
      view: showcase
      # For Showcase view, flip alternate rows?
      flip_alt_rows: false
  - block: contact
    id: contact
    content:
      title: Contact
      subtitle:
      text: 
      email: test@example.org
      phone: 888 888 88 88
      address:
        street: 450 Serra Mall
        city: Stanford
        region: CA
        postcode: '94305'
        country: United States
        country_code: US
      contact_links:
        - icon: video
          icon_pack: fas
          name: Zoom Me
          link: 'https://zoom.com'
      # Automatically link email and phone or display as text?
      autolink: true
    design:
      columns: '2'
---
