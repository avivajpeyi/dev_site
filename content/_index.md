---
# Leave the homepage title empty to use the site title
title:
date: 2022-10-24
type: landing

sections:
  - block: about.biography
    id: about
    content:
      username: admin
      text: 
  - block: collection
    id: publications
    content:
      title: Publications
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
    headless: true  # This file represents a page section.
    id: games
    content:
      title: Games
      text: |-
        <span>I love participating in game jams and making games in my spare time. Here are some that I had fun working on.</span><br/><span>  <br/> </span>
      filters:
        folders:
          - games
      default_button_index: 0
      buttons:
        - name: All
          tag: '*'
        - name: Procedually Generated
          tag: proc-gen
        - name: Voice Controlled
          tag: voice
        - name: Machine Learning
          tag: ML-Agent 
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '2'
      view: masonry
      # For Showcase view, flip alternate rows?
      flip_alt_rows: true
  - block: contact
    id: contact
    content:
      title: Contact
      subtitle:
      text: 
      email: avi.vajpeyi@auckland.ac.nz
      phone: +64 22 543 1418
      address:
        street: Building 303, Rm 305
        city: 38 Princes Street
        region: Auckland
        postcode: '1010'
        country: New Zealand
        country_code: ZN     
      office_hours:
        - 'Weekdays 8:30 to 4:30'
      coordinates:
        latitude: '-36.852387'
        longitude: '174.768083'
      contact_links:
        - icon: github
          icon_pack: fab
          name: Github
          link: 'https://github.com/avivajpeyi'
        - icon: gitlab
          icon_pack: fab
          name: Git-LIGO
          link: 'https://git.ligo.org/avi.vajpeyi'
        - icon: gamepad
          icon_pack: fas
          name: Try my games
          link: 'https://avivajpeyi.itch.io/'
      # Automatically link email and phone or display as text?
      autolink: true
    design:
      columns: '2'
---
