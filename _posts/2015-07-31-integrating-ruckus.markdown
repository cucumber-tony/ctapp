---
layout: post
title:  Setting up a splash page for a Ruckus ZD network
date:   2015-07-31
categories: documentation
keywords: Ruckus captive portal, Ruckus splash page hosting, Ruckus splash page not working, Ruckus splash page template, Ruckus splash page free, Ruckus splash page html
---

In order to use Ruckus with the Cucumber Tony logins and radius, you'll need to make some changes in your Ruckus Zone Director.

Add your <b>Access Points</b> into Cucumber Tony by adding their MAC addresses.

You can find the MAC addresses under the <b>Monitor</b> tab of the </b>Zone Director</b>. You should add the MAC addresses of all access points that are going to be in use with the splash pages.

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/ap-screen.png">
</div>

When you have added these to Cucumber Tony, go into the <b>settings</b> page of your <b>box</b>, scrolling down for the section labelled <b>PolkaSpots Settings</b>.

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/not-ps-box.png">
</div>

<b>Untick</b> I'm a PolkaSpots Box, and select <b>Ruckus</b> from the droplist then update your changes.

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/ruckus-box.png">
</div>

Head back to <b>boxes</b> and click on the box you just set to a non PolkaSpots Box. You should now see a green <b>Radius Settings</b> button.

Clicking this will open a box with details that you will need to refer back to at later stages.

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/radius-info.png">
</div>

Connect to your Zone Director device and click on the <b>Configure</b> tab, then click <b>AAA Servers</b> on the sidebar, and then click <b>Create New</b>.

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/aaa-servers.png">
</div>

A selection of fields will appear, which will need to be filled in with the following information:
<ul>
<li>Name: e.g PolkaRad</li>
<li>Type: RADIUS</li>
<li>Auth Method: PAP</li>
<li>IP Address: (Refer to CT Radius Settings - Radius Servers)</li>
<li>Port: 1812</li>
<li>Secret: (Refer to CT Radius Settings - Radius Secret)</li>
<li>Confirm Secret: (As above)</li>
</ul>

And then click <b>OK</b>

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/radius-servers.png">
</div>

Complete the same again, creating a <b>new entry</b>, but this time with the following information:

<ul>
<li>Name: PolkaAcc</li>
<li>Type: RADIUS Accounting</li>
<li>IP Address: (Refer to CT Radius Settings - Radius Server)</li>
<li>Port: 1813</li>
<li>Shared Secret: (Refer to CT Radius Settings - Radius Secret)</li>
<li>Confirm Secret: (As above)</li>
</ul>

And then save your changes by clicking <b>OK</b>

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/save-server.png">
</div>

Click <b>Hotspot Services</b> on the sidebar, and then click <b>Create New</b>

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/new-hotspot.png">
</div>

A selection of fields will appear which will need to be filled with the following information:

<ul>
<li>Name: e.g PolkaSpotsHS</li>
<li>Login Page: (Refer to CT Radius Settings - Splash URL)</li>
<li>Authentication Server: PolkaRad (the RADIUS entry you created in step 3)</li>
<li>Accounting Server: PolkaAcc (the RADIUS Accounting entry you created in step 4)</li>
</ul>

Then click on <b>Walled Gardens</b>.

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/walled-gardens.png">
</div>

You will now see the Walled Garden entries. Click <b>Create New</b> to add the entries needed

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/new-wg.png">
</div>

add the following to the <b>Walled garden</b>:

You can find the most up to date list of <b><span data-elevio-article="18499">Walled Gardens here.</span></b>

And then save the changes by clicking <b>OK</b>

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/save-wg.png">
</div>

Click <b>WLANs</b> on the sidebar, and under WLANs click <b>Create New</b>

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/new-wlan.png">
</div>

A selection of fields will now have appeared which will need to be filled with the following information:

<ul>
<li>Name/ESSID: e.g PolkaFly</li>
<li>ESSID: e.g PolkaFly</li>
<li>Type: Hotspot Service (WISPr)</li>
<li>Hotspot Services: PolkaSpotsHS (the Hotspot Service created in step 6)</li>
</ul>

Then save your changes by clicking <b>OK</b>

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/save-wlan.png">
</div>

Because there are no options to change these settings in the GUI, you will need to <b>SSH</b> in to the Ruckus <b>ZoneDirector</b>, which is done by entering the following:

<blockquote>ssh root@DEVICE_IP</blockquote>

Replace DEVICE_IP with the IP of your zone director, and login with your username and password that you used to log into the ZoneDirector.

Once you have established an SSH connection, enter the commands below, one by one:

<blockquote>
enable<br>
config<br>
wlan "PolkaFly" <b>(name of essid in step 10)</b><br>
called-station-id-type ap-mac<br>
nasid-type mac-addr<br>
end
</blockquote>

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/ssh-console.png">
</div>

Get MAC address of zonedirector from the zonedirector dashboard

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/zone-mac.png">
</div>

<b>Change the box from a PolkaSpots box to a Ruckus box</b>

This will update the icon for you and activate the radius settings.

Head to your boxes in Cucumber Tony and go into their settings.

Then scroll down under Advanced Settings, and click on edit advanced settings.

<b>In the NAS field, enter the MAC address of the ZoneDirector that you collected earlier.

It is important the MAC is separated with hyphens because that's what Ruckus send us.
e.g 11-22-33-44-55-66</b>

<div class="text-center">
<img src="/images/community/tutorials/ruckus-zd-guide/nas-id.png">
</div>

Then scroll to the bottom of the page and click update.

Do this for all of your Ruckus Access Points.

<br>

##Troubleshooting & Questions
###Can't login - Stuck in a Loop <hr>
You can see the login page but when you login, it sends you back to the login page. Here's what you can do:

- &#8226; Swap the access type to clickthrough to rule out password issues
- &#8226; Ensure your radius secrets match the ones in your zone director radius settings
- &#8226; Ensure your Nas-Id from your Cucumber Wi-Fi account matches the one in your zone director
- &#8226; Make sure that your SSID entered through SSH matches the one you entered into your zone director

If you do connect with an incorrect secret/nas_id then our servers will lock you out as a security measure.

If this happens to you, just drop us a message.

###Can I use multiple splash pages? <hr>
Yes you can, and there are three ways to do it in CT.

**1. Tags**

We can intercept the tags sent in the url and present a page depending on them. This is currently in alpha.

**2. Hard coded url**

You can hardcode the specific splash pages to your Splash Page URLs.

To do this you will need your splash page's unique ID from CT.
You can find this in Location > Settings > Scroll down to Address Details > Unique ID

Then, add this to the Splash URL you entered in the ZD:

http://app.my-wifi.co/long-id-of-the-splash

**3. CT Zones**

This is the quickest of the options, simply create two zones in CT and add an SSID to each zone (which will have the splash page you want assosciated with it).

Then assign your boxes to the right zone.

This way you don't have to change anything in the ZD.

<hr>

*Cucumber Tony is a cloud based WiFi management platform for businesses. The firmware gives consumer-grade WiFi access points enterprise-like capabilities.Or you can utlise the captive portal solution with your existing infrastucture. Create a free account and check it here <a href="https://cucumberwifi.io">cucumberwifi.io</a>*

<div class="text-center">

<h2>Come join Cucumber</h2>

<a href="https://my.ctapp.io/#/create" class="button success dst">Sign Up</a><br>

<p>Sign-up for Cucumber Tony, it's free for unlimited access points :)</p>

<hr>

</div>
