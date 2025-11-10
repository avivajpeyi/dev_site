---
# Leave the homepage title empty to use the site title
title: ''
date: 2022-10-24
type: landing

design:
  spacing: '6rem'

sections:
  - block: resume-biography-3
    id: about
    content:
      username: admin
      text: >-
        I am a postdoctoral research fellow at the University of Auckland. I use
        Bayesian inference and creative coding to understand how black holes form,
        merge, and echo across the Universe. I am a member of the
        [LIGO-Virgo-KAGRA Collaboration](https://www.ligo.caltech.edu/) and the
        [OzGrav Centre of Excellence](https://www.ozgrav.org/).
      button:
        text: Download CV
        url: uploads/resume.pdf
      headings:
        about: ''
        education: ''
        interests: ''
    design:
      css_class: hbx-bg-gradient
      avatar:
        size: medium
        shape: circle

  - block: markdown
    content:
      title: '📚 Research Focus'
      text: |-
        My work builds Bayesian and simulation-based inference tools for
        gravitational-wave astronomy. I am especially interested in extreme-mass
        ratio systems, stochastic backgrounds, and data-driven pipelines that let
        us rapidly test astrophysical formation scenarios.
    design:
      columns: '1'

  - block: collection
    id: posts
    content:
      title: Recent Posts
      subtitle: Notes from research, workshops, and side projects.
      page_type: posts
      count: 6
      filters:
        folders:
          - posts
    design:
      view: card
      columns: 2

  - block: collection
    id: games
    content:
      title: Games & Experiments
      text: >-
        I love participating in game jams and prototyping with Godot and Unity.
        These projects blur art, physics, and play.
      filters:
        folders:
          - games
      count: 12
    design:
      view: card
      columns: 3

  - block: collection
    id: publications
    content:
      title: Publications
      text: "Selected peer-reviewed articles and preprints."
      filters:
        folders:
          - publication
    design:
      view: citation
      columns: 2

  - block: markdown
    id: contact
    content:
      title: Contact
      text: |-
        **Email:** [avi.vajpeyi@auckland.ac.nz](mailto:avi.vajpeyi@auckland.ac.nz)  
        **Phone:** +64 22 543 1418  
        **Office:** Building 303, Room 305, University of Auckland, New Zealand  
        **Hours:** Weekdays 08:30–16:30 NZT  

        [GitHub](https://github.com/avivajpeyi) · [LIGO GitLab](https://git.ligo.org/avi.vajpeyi) · [Itch.io](https://avivajpeyi.itch.io/)
---
