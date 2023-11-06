# Adaptive Mushroom
A minimalistic Mushroom-based dashboard for Home Assistant. No more need to maintain multiple dashboards! With this setup, you can create a single minimalistic-looking dashboard for mobile/desktop and for light/dark mode.

[![Buy me a beer](https://img.shields.io/badge/Buy%20me%20a-beer-yellow?logo=buy-me-a-coffee)](https://www.buymeacoffee.com/sganoud)

## Installation
### 1. Installing dependencies
You will need to install the following frontend dependencies (using HACS or manually). Some are mandatory for the adaptive layout to work properly, others are used only for specific cards in my personal dashboard. If you want to use some of my custom cards you will likely need button-card and stack-in-card.

Dependency | Mandatory | Use-case |
--- | --- | --- |
layout-card | yes | Determines the layout |
mushroom | yes | For the basic cards |
lovelace-card-mod | yes | Modifies card appearances |
decluttering-card | yes | Re-use card templates to declutter lovelace config |
button-card | recommended | For the custom-made cards |
stack-in-card | recommended | Combines several cards into one |
mini-graph-card | no | For displaying a simple graph |
apexcharts-card | no | For displaying advanced graphs and charts |
bar-card | no | For displaying a bar |
hourly-weather-card | no | For displaying weather forecasts as a bar |
swipe-card | no | For swiping between cards |

### 2. Installing the theme
The theme defines the dashboard appearance and some variables necessary for light/dark mode. To make sure my theme is not overwritten by a Mushroom update, I put the file in a separate folder in the `themes` directory called `Adaptive Mushroom` next to the `Mushroom` folder that is there if you're using that theme.
1. Copy the `themes/adaptive-mushroom` folder to your Home Assistant `themes` directory.
2. Add the following lines to your `configuration.yaml`:
```yaml
frontend:
  themes: !include_dir_merge_named themes
```
3. Restart Home Assistant.
4. After restarting, go to your profile and select the Adaptive Mushroom theme from the drop-down menu.

*You can also change your original Mushroom theme if you have it installed already, but I'm not sure if it remains untouched inbetween Mushroom updates. Don't forget to refresh your theme with the `frontend.reload_themes` service.*

### 3. Configuring a new dashboard
1. Create a new dashboard and visit it.
2. Click on the 3 dots in the top right corner and hit `Edit dashboard`, confirm that you'll take control and check the box to start with an empty dashboard.
3. Click the 3 dots again, `Edit dashboard` and then `Raw configuration editor`.
4. Remove the code that's there.
5. Open the `lovelace.yaml` file on Github and copy its decluttering templates, starting from the first line `decluttering_templates:` to `views:`.
6. Paste the code in the `Raw configuration editor` in Home Assistant.
7. Below this code you just pasted, add the following lines (pay attention to the `#` commented lines):
```yaml
views:
  - theme: Adaptive Mushroom
    title: Home
    icon: mdi:home-assistant
    path: home
    visible:
      - user: 12345678901234567980  # In step 9 you will edit the view in the UI to change visibility
    type: custom:grid-layout
    layout:
      grid-template-columns: 0em 74px 30% 30% 30% auto
      grid-template-rows: grid-title-row-height auto auto auto
      grid-gap: 0.5em
      grid-template-areas: |  # You can define your own grid here later, but this one works as well
        ". . title title people ."
        ". nav left1 center1 right1 ."
        ". nav left2 center2 right2 ."
      mediaquery:
        '(max-width: 400px)':
          grid-template-columns: 3% 94% 3%
          grid-template-rows: auto
          grid-gap: 0em
          grid-template-areas: |  # You can define your own grid here later, but this one works as well
            ". title ."
            ". people ."
            ". left1 ."
            ". left2 ."
            ". center1 ."
            ". center2 ."
            ". right1 ."
            ". right2 ."
            "footer footer footer"
    badges: []
    cards:
      - type: vertical-stack
        cards:
          - type: custom:mushroom-title-card
            title: ' '
          - type: custom:decluttering-card
            template: side-nav
            variables:
              - dashboard-name: dashboard-view  # Edit to your dashboard URL
              - active-view: home  # Edit to the active dashboard view
        view_layout:
          grid-area: nav
          show:
            mediaquery: '(min-width: 400px)'
      - type: custom:decluttering-card
        template: bottom-nav
        variables:
          - dashboard-name: dashboard-view  # Edit to your dashboard URL
          - active-view: home  # Edit to the active dashboard view
        view_layout:
          grid-area: footer
          show:
            mediaquery: '(max-width: 400px)'
      - type: custom:mod-card
        view_layout:
          grid-area: title
        card:
          type: custom:mushroom-title-card
          title: |-
            {% set time = now().hour %} {% if (time >= 18) %} 
             Goedenavond, {{user}}
            {% elif (time >= 12) %}
             Goedemiddag, {{user}}
            {% elif (time >= 5) %}
             Goedemorgen {{user}}
            {% else %}
             Slaap lekker, {{user}}!
            {% endif %}
          subtitle: Welcome to Adaptive Mushroom!
        card_mod:
          style:
            mushroom-title-card$: |
              h1 {
                --title-font-size: 26px;
                --title-font-weight: bold;
                --title-line-height: 1;
              }
              h2 {
                --title-font-size: 16px;
              }
              .header {
                --title-padding: 12px 12px 0px;
              }
```
7. Click on `Save` and then `X`. If you left in comments, they won't be saved, but that's ok.
8. You should now have a title and a navigation menu on the left of the screen for tablet/desktop and on the bottom of the screen for mobile (please note that the bottom navigation footer needs more content to stay at the bottom of the page).
9. Edit the view in the UI to change its visibility.

### 4. Configuring the other cards and views
Now it's time to start adding cards to your dashboard as you wish! I find it easiest to use vertical stacks to organize my layout. To place them in a specific area of the layout, add the following code to a vertical stack (or any card):
```yaml
type: vertical-stack
cards:
  - ...
view_layout:
  grid-area: YOUR_AREA_NAME_HERE
```
You can always reorganize the areas in your layout by editing the content of the `custom:grid-layout` card that is at the base of the dashboard view.

The navigation menu will start working once you add more views. Remember to change the decluttering templates of both navigation components (called `side-nav` and `bottom-nav`) to suit your views.

## ToDo's
[] Add screenshots
[] Add lovelace.yaml of other views
