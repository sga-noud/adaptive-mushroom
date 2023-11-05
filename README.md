# Adaptive Mushroom
A minimalistic Mushroom-based dashboard for Home Assistant. No more need to maintain multiple dashboards! With this setup, you can create a single minimalistic-looking dashboard for mobile/desktop and for light/dark mode.

## Installation
### 1. Installing dependencies
You will need to install the following frontend dependencies (using HACS or manually). Some are mandatory for the adaptive layout to work properly, others are used only for specific cards in my personal dashboard. Personally, I use Mushroom cards a basis, but for more advanced cards I combine those with button-card and stack-in-card (similar to, but easier than, the Minimalist approach).

Dependency | Mandatory | Use-case |
--- | --- | --- |
layout-card | yes | Determines the layout |
mushroom | yes | For the basic cards |
button-card | yes | For the custom-made cards |
lovelace-card-mod | yes | Modifies card appearances |
stack-in-card | yes | Combines several cards into one |
decluttering-card | recommended | Re-use card templates to declutter lovelace config |
mini-graph-card | no | For displaying a simple graph |
apexcharts-card | no | For displaying advanced graphs and charts |
bar-card | no | For displaying a bar |
hourly-weather-card | no | For displaying weather forecasts as a bar |
swipe-card | no | For swiping between cards |

### 2. Installing the theme
1. Copy the `themes` folder in your Home Assistant configuration directory.
2. Restart Home Assistant.
3. After restarting, go to your profile and select the Mushroom Custom Theme from the drop-down menu.

*You can also change your original Mushroom theme if you have it installed already. You can use the `frontend.reload_themes` service to refresh it.*

### 3. Configuring a new dashboard
1. Create a new dashboard, go to it, click on the 3 dots in the top right corner and hit `Edit dashboard` and then `Raw configuration editor`.
2. Open the `lovelace.yaml` file with a text editor and copy its content.
3. Paste the code in the `Raw configuration editor` in Home Assistant.
4. Click on `Save` and then `X`.
5. You should now have a working navigation menu on the left of the screen for tablet/desktop and on the bottom of the screen for mobile (please note that the bottom navigation footer needs more content to stay at the bottom of the page).

### 4. Configuring the other cards and views
Add cards to your dashboard as you wish! I find it easiest to use vertical stacks to organize my layout. To place them in a specific area of the layout, add the following code to a vertical stack:
```yaml
type: vertical-stack
cards:
  - ...
view_layout:
  grid-area: YOUR_AREA_NAME_HERE
```
You can always reorganize the areas in your layout by editing the content of the `custom:grid-layout` card that is at the base of the dashboard view.
