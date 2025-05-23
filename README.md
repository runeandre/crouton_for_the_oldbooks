# "crouton" for old Chromebooks (MODIFIED)

This version of crouton has been modified to work with "Chrome OS 75.0.3770.144 (2019)" and old Chromebooks.
<br>It has been tested on a "Samsung Chromebook 2 (XE503C32 - Peach Pi)", but should work on other Chromebooks as well.

<b>The important change in this repository is reverting the debootstrap version to "1.0.128+nmu2" as the newer version doesn't work on the old "Samsung Chromebook 2 (XE503C32 - Peach Pi)".</b>

<b>Ubuntu 20.04 Focal is the distro and version used.</b>
<br>This is the newest version that worked so far in my testing.
<br>Older versions also doesn't work as they tend to have GPU related issues.

The commit where debootstrap was updated, and crouton stopped working on my old Chromebook: https://github.com/dnschneid/crouton/commit/4c6d9e23ca23ad3d17e4d7647eebe7374e7e975d

## Instructions
1. Boot the Chromebook with developer mode enabled (Google how to do this).
2. Open a shell: Ctrl+Alt+T -> Type 'shell' and press enter.
3. Open the home folder: 'cd'
4. Open the Downloads folder: 'cd Downloads'
5. Download crouton to the Chromebook: 'curl -L https://github.com/runeandre/crouton_for_the_oldbooks/raw/refs/heads/master/installer/crouton --output crouton'
6. Copy crouton to bin folder: 'sudo install -Dt /usr/local/bin -m 755 crouton'
7. Install Ubuntu 20.04 (Focal) with XFCE: 'sudo crouton -t xfce -r focal'
8. Start Ubuntu: 'sudo startxfce4'"

List Linux versions: 'crouton -r list'
<br>Delete all installed Linux: 'sudo delete-chroot -a'

The instructions are basically the same as the original.
<br><b>The main difference is using this as the source, and that "focal" is specified as the Ubuntu version to use.</b>

You can also use the following script to modify crouton yourself and host everything locally on your Linux PC: https://github.com/runeandre/crouton_for_the_oldbooks_script

![ChromeOS 75 with Xubuntu 20.04](./assets/ChromeOS_75_with_Xubuntu_20.04.png)

---


# crouton: Chromium OS Universal Chroot Environment

crouton is a set of scripts that bundle up into an easy-to-use,
Chromium OS-centric chroot generator. Ubuntu, Debian, and Kali are supported
(using debootstrap behind the scenes), but "Chromium OS Debian and Ubuntu (plus
one distro) EOL'd Chroot Environment" doesn't acronymize as well (crodupodece is
admittedly pretty fun to say, though).

### 🪦 crouton is now end-of-life 🪦

All good things must come to an end, and considering
 * Chromium OS's introduction of increasingly strict shell safeguards,
 * the [change in cras's build tools](https://github.com/runeandre/crouton_for_the_oldbooks/issues/4958),
 * the [removal of manifest v2 extensions](https://github.com/runeandre/crouton_for_the_oldbooks/pull/5094),
 * the [removal of PNaCl, breaking xiwi](https://github.com/runeandre/crouton_for_the_oldbooks/issues/5130),
 * oh, and Chromium OS being replaced by Android

there's really not much to gain from continued development. Put another way, the
proverbial mixed salad is just about out of tasty crunchy bits, and the
remaining morsels have gone a bit stale. And for some reason someone's about to
swap all the lettuce out for onion rings? Point is, it's time to stop mixing
dressings. Unless it's ranch, I guess, since that goes with just about anything.
But this is crouton, not some Ready-for-Android Native Chroot Host, alas.

Anyway, this means that:
 * The repo is now locked, and no further changes will be considered.
 * Eventually someone will want the latest Ubuntu added to the release list. See
   [this commit](https://github.com/runeandre/crouton_for_the_oldbooks/commit/6d80f57b91c39d10b29fde861aac5a2b5b9b3910)
   for an example of how to do it on your own copy.
 * Sometime around July 2025, the GitHub project will be archived, making the
   issue tracker, discussions, and wiki read-only.
 * For the safety of users and stability of crouton's functionality for those on
   EOL devices, offers to take over the dnschneid/crouton repo or extension will
   be declined, and requests to change link destinations will be rejected.

If you have an EOL device, though, crouton is still a great match for you!
 * Chromium OS version 110 and earlier, everything should work! Breathe new life
   into your old devices!!
 * Starting 111, crouton can't compile cras, so audio devices cannot be shared
   with Chromium OS
 * Starting 117, sudo in crosh is disabled and you'll need to use VT-2
 * Starting 133 (139 for enterprise devices), manifest v2 is disabled and you
   won't be able to run the extension (easy switching, clipboard sync, xiwi)
 * Also starting 133 (139 for enterprise devices), pnacl is disabled, so even if
   you somehow got the extension working, xiwi won't function
 * Beyond that, it's anybody's guess as to what will break

## Moving right along...

:warning: **Steps to install crouton have changed!**  :warning:

Chromium OS has introduced several security features over the years that impede
the installation and usage of crouton. If your device is no longer receiving
updates, the steps below will likely work for you. However, if you are still
having trouble, please try the [community-maintained instructions](https://github.com/runeandre/crouton_for_the_oldbooks/wiki/Updated-Installation-Instructions-for-Crouton).

In addition, goo.gl is going away! That means the goo.gl/fd3zc you know and love
has been replaced with [https://github.com/runeandre/crouton_for_the_oldbooks/raw/refs/heads/master/installer/crouton](https://github.com/runeandre/crouton_for_the_oldbooks/raw/refs/heads/master/installer/crouton). That's a zero at
the end, if you were wondering. Both just point to
[github](https://raw.githubusercontent.com/dnschneid/crouton/master/installer/crouton),
so you can always just memorize the full link instead, which (fun fact) does not
include any numbers at all!

## "crouton"...an acronym?

It stands for _ChRomium Os Universal chrooT envirONment_
...or something like that. Do capitals really matter if caps-lock has been
(mostly) banished, and the keycaps are all lower-case?

Moving on...


## Who's this for?

Anyone who wants to run straight Linux on their Chromium OS device, and doesn't
care about physical security. You're also better off having some knowledge of
Linux tools and the command line in case things go funny, but it's not strictly
necessary.


## What's a chroot?

Like virtualization, chroots provide the guest OS with their own, segregated
file system to run in, allowing applications to run in a different binary
environment from the host OS. Unlike virtualization, you are *not* booting a
second OS; instead, the guest OS is running using the Chromium OS system. The
benefit to this is that there is zero speed penalty since everything is run
natively, and you aren't wasting RAM to boot two OSes at the same time. The
downside is that you must be running the correct chroot for your hardware, the
software must be compatible with Chromium OS's kernel, and machine resources are
inextricably tied between the host Chromium OS and the guest OS. What this means
is that while the chroot cannot directly access files outside of its view, it
*can* access all of your hardware devices, including the entire contents of
memory. A root exploit in your guest OS will essentially have unfettered access
to the rest of Chromium OS.

...but hey, you can run [TuxRacer](https://en.wikipedia.org/wiki/Tux_Racer)!


### What about dem crostinis though?

[Crostini](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/containers_and_vms.md)
is an official project within Chromium OS to bring the Linux shell and apps to
the platform *in verified mode* with clean integration, multi-layered security,
and all the polish you expect from Chromium OS proper.

That means compared to crouton, Crostini has official support, competent
engineers, and code that looks a little less like ramen.  crouton, in its
defense, has wider device compatibility, enables direct hardware access, and is
named after an objectively tastier bread-based food item.

There's a solid community on [Reddit](https://www.reddit.com/r/Crostini/) if
you'd like to try Crostini out.  If it works for you -- great!  No hard
feelings.  If in the end you decide that crouton suits you better, read on!

Note: you can't get the best of both worlds by installing crouton inside of
Crostini.  The technology (and life itself) just doesn't work that way.  Not to
mention a crouton Crostini would look ridiculous and be impossible to eat
without getting bits everywhere.


## Prerequisites

You need a device running Chromium OS that has been switched to developer mode.

For instructions on how to do that, go to [this Chromium OS wiki page](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices),
click on your device model and follow the steps in the *Entering Developer Mode*
section.

Note that developer mode, in its default configuration, is *completely
insecure*, so don't expect a password in your chroot to keep anyone from your
data. crouton does support encrypting chroots, but the encryption is only as
strong as the quality of your passphrase. Consider this your warning.

It's also highly recommended that you install the [crouton extension](https://chromewebstore.google.com/detail/crouton-integration/gcpneefbbnfalgjniomfjknbcgkbijom),
which, when combined with the `extension` or `xiwi` targets, provides much 
improved integration with Chromium OS.

That's it! Surprised?


## Usage

crouton is a powerful tool, and there are a *lot* of features, but basic usage
is as simple as possible by design.

If you're just here to use crouton, you can grab the latest release from
[https://github.com/runeandre/crouton_for_the_oldbooks/raw/refs/heads/master/installer/crouton](https://github.com/runeandre/crouton_for_the_oldbooks/raw/refs/heads/master/installer/crouton). Download it, pop open a
shell (Ctrl+Alt+T, type `shell` and hit enter), make the installer executable
with `sudo install -Dt /usr/local/bin -m 755 ~/Downloads/crouton`, then launch
it with `sudo crouton` to see the help text. See the "examples" section for some
usage examples.

If you're modifying crouton, you'll probably want to clone or download the repo
into a subdirectory of `/usr/local` and then either run `installer/main.sh`
directly, or use `make` to build your very own `crouton`. You can also download
the latest release, install it as above and run `crouton -x` to extract out the
juicy scripts contained within, but you'll be missing build-time stuff like the
Makefile. You also need to remember to place the unbundled scripts somewhere in
`/usr/local` in order to be able to execute them.

crouton uses the concept of "targets" to decide what to install. While you will
have apt-get in your chroot, some targets may need minor hacks to avoid issues
when running in the chrooted environment. As such, if you expect to want
something that is fulfilled by a target, install that target when you make the
chroot and you'll have an easier time.  Don't worry if you forget to include a
target; you can always update the chroot later and add it. You can see the list
of available targets by running `crouton -t help`.

Once you've set up your chroot, you can easily enter it using the
newly-installed `enter-chroot` command, or one of the target-specific
start\* commands. Ta-da! That was easy.


## Examples

### The easy way (assuming you want an Ubuntu LTS with Xfce)

  1. Download `crouton`
  2. Open a shell (Ctrl+Alt+T, type `shell` and hit enter)
  3. Copy the installer to an executable location by running
     `sudo install -Dt /usr/local/bin -m 755 ~/Downloads/crouton`
  4. Now that it's executable, run the installer itself: `sudo crouton -t xfce`
  5. Wait patiently and answer the prompts like a good person.
  6. Done! You can jump straight to your Xfce session by running
     `sudo enter-chroot startxfce4` or, as a special shortcut, `sudo startxfce4`
  7. Cycle through Chromium OS and your running graphical chroots using
     Ctrl+Alt+Shift+Back and Ctrl+Alt+Shift+Forward.
  8. Exit the chroot by logging out of Xfce.

### With encryption!

  1. Add the `-e` parameter when you run crouton to create an encrypted chroot
     or encrypt a non-encrypted chroot.
  2. You can get some extra protection on your chroot by storing the decryption
     key separately from the place the chroot is stored. Use the `-k` parameter
     to specify a file or directory to store the keys in (such as a USB drive or
     SD card) when you create the chroot. Beware that if you lose this file,
     your chroot will not be decryptable. That's kind of the point, of course.

### Hey now, Ubuntu 16.04 is pretty old; I'm young and hip

  1. The `-r` parameter specifies which distro release you want to use.
  2. Run `crouton -r list` to list the recognized releases and which distros
     they belong to.

### Wasteful redundancies are wasteful: one clipboard, one browser, one window

  1. Install the [crouton extension](https://chromewebstore.google.com/detail/crouton-integration/gcpneefbbnfalgjniomfjknbcgkbijom)
     into Chromium OS.
  2. Add the `extension` or `xiwi` version to your chroot.
  3. Try some copy-pasta, or uninstall all your web browsers from the chroot.

*Installing the extension and its target gives you synchronized clipboards, the
option of using Chromium OS to handle URLs, and allows chroots to create
graphical sessions as Chromium OS windows.*

### I don't always use Linux, but when I do, I use CLI

  1. You can save a chunk of space by ditching X and just installing
     command-line tools using `-t core` or `-t cli-extra`
  2. Enter the chroot in as many crosh shells as you want simultaneously using
     `sudo enter-chroot`
  3. Use the [Crosh Window](https://chromewebstore.google.com/detail/crosh-window/nhbmpbdladcchdhkemlojfjdknjadhmh)
     extension to keep Chromium OS from eating standard keyboard shortcuts.
  4. If you installed cli-extra, `startcli` will launch a new VT right into the
     chroot.

### A new version of crouton came out; my chroot is therefore obsolete and sad

  1. Exit the chroot if you have it open.
  2. If you haven't already, download `crouton`, and copy it so it works:
     `sudo install -Dt /usr/local/bin -m 755 ~/Downloads/crouton`
  3. Update your chroot with `sudo crouton -u -n chrootname`. It will update
     all installed targets.

### I want to open my desktop in a window or a tab but I don't have the 'xiwi' target/xmethod.

  1. Add 'xiwi' or any other target to an existing chroot with the `-u` option:
     `sudo crouton -t xiwi -u -n chrootname`

  This will also make 'xiwi' the default xmethod.

  2. If you want to keep the 'xorg' xmethod as the default then pick it first:
     `sudo sh crouton -t xorg,xiwi -u -n chrootname`

### A backup a day keeps the price-gouging data restoration services away

  1. `sudo edit-chroot -b chrootname` backs up your chroot to a timestamped
     tarball in the current directory. Chroots are named either via the `-n`
     parameter when created or by the release name if -n was not specified.
  2. `sudo edit-chroot -r chrootname` restores the chroot from the most recent
     timestamped tarball. You can explicitly specify the tarball with `-f`
  3. If your machine is new, powerwashed, or held upside-down and shaken, you
     can use the crouton installer to restore a chroot and relevant scripts:
     `sudo crouton -f mybackup.tar.gz`

*Unlike with Chromium OS, the data in your chroot isn't synced to the cloud.*

### This chroot's name/location/password/existence sucks. How to fix?

  1. Check out the `edit-chroot` command; it likely does what you need it to do.
  2. If you set a Chromium OS root password, you can change it with
     `sudo chromeos-setdevpasswd`
  3. You can change the password inside your chroot with `passwd`

### I want to install the chroot to another location

  1. Use `-p` to specify the directory in which to install the chroot and
     scripts. Be sure to quote or escape spaces.
  2. When entering the chroot for the first time each boot, you will first need
     to ensure the place you've installed the scripts is in a place that allows
     executables to run. Determine the mountpoint by running
     `df --output=target /path/to/enter-chroot`, then mark the mount exec with
     `sudo mount -o remount,exec /path/to/mountpoint`.
  3. You can then launch the chroot by specifying the full path of any of the
     enter-chroot or start* scripts (i.e. `sudo /path/to/enter-chroot`), or use
     the `-c` parameter to explicitly specify the chroots directory.

*If for some reason you have to run the installer without touching the local
disk, you can (for the time being) run
`curl -fL https://github.com/runeandre/crouton_for_the_oldbooks/raw/refs/heads/master/installer/crouton | sudo sh -s -- options_for_crouton_installer`.
Note that this will definitely break in the near future, so don't depend on it.*

### Downloading bootstrap files over and over again is a waste of time

  1. Download `crouton`
  2. Open a shell (Ctrl+Alt+T, type `shell` and hit enter)
  3. Copy the installer to an executable location by running
     `sudo install -Dt /usr/local/bin -m 755 ~/Downloads/crouton`
  4. Now that it's executable, use the installer to build a bootstrap tarball:
     `sudo crouton -d -f ~/Downloads/mybootstrap.tar.bz2`
  5. Include the `-r` parameter if you want to specify for which release to
     prepare a bootstrap.
  6. You can then create chroots using the tarball by running
     `sudo crouton -f ~/Downloads/mybootstrap.tar.bz2`. Make sure you also
     specify the target environment with `-t`.

*This is the quickest way to create multiple chroots at once, since you won't
have to determine and download the bootstrap files every time.*

### Targets are cool. Abusing them for fun and profit is even cooler

  1. You can make your own target files (start by copying one of the existing
     ones) and then use them with any version of crouton via the `-T` parameter.

*This is great for automating common tasks when creating chroots.*

### Help! I've created a monster that must be slain!

  1. The delete-chroot command is your sword, shield, and only true friend.
     `sudo delete-chroot evilchroot`
  2. It's actually just a shortcut to `sudo edit-chroot -d evilchroot`, which I
     suppose makes it a bit of a deceptive Swiss Army knife friend...still good?


## Tips

  * Chroots are cheap! Create multiple ones using `-n`, break them, then make
    new, better ones!
  * You can change the distro mirror from the default by using `-m`
  * Want to use a proxy? `-P` lets you specify one (or disable it).
  * A script is installed in your chroot called `brightness`. You can assign
    this to keyboard shortcuts to adjust the brightness of the screen (e.g.
    `brightness up`) or keyboard (e.g. `brightness k down`).
  * Multiple monitors will work fine in the chroot, but you may have to switch
    to Chromium OS and back to enable them.
  * You can make commands run in the background so that you can close the
    terminal. This is particularly useful for desktop environments: try running
    `sudo startxfce4 -b`
  * Want to disable Chromium OS's power management? Run `croutonpowerd -i`
  * Only want power management disabled for the duration of a command?
    `croutonpowerd -i command and arguments` will automatically stop inhibiting
    power management when the command exits.
  * Have a Pixel or two or 4.352 million? `-t touch` improves touch support.
  * Want to share some files and/or folders between Chromium OS and your chroot?
    Check out the `/etc/crouton/shares` file, or read all about it in the wiki.
  * Want more tips? Check the [wiki](https://github.com/runeandre/crouton_for_the_oldbooks/wiki).


## Issues?

Running another OS in a chroot is a pretty messy technique (although it's hidden
behind very pretty scripts), and while these scripts are relatively mature,
Chromium OS is changing all the time so problems are not surprising. Check the
issue tracker and file a bug if your issue isn't there. When filing a new bug,
include the output of `croutonversion` run from inside the chroot or, if you
cannot mount your chroot, include the output of `cat /etc/lsb-release` from Crosh.


## I want to be a Contributor!

That's great!  But before your code can be merged, you'll need to have signed
the [Individual Contributor License Agreement](https://cla.developers.google.com/clas/new?kind=KIND_INDIVIDUAL&domain=DOMAIN_GOOGLE).
Don't worry, it only takes a minute and you'll definitely get to keep your
firstborn, probably.  If you've already signed it for contributing to Chromium
or Chromium OS, you're already done.

If you don't know what to do with your time as an official Contributor, keep in
mind that crouton is maintenance-only and will only be accepting a limited amount
of changes.  That having been said, here's some suggestions:

  * Really like a certain desktop environment? Fork crouton, add the target, and
    let people know in the discussions area.
  * Is your distro underrepresented? Want to contribute to the elusive and
    mythical beast known as "croagh"? Fork crouton, add the distro, and people
    will come.
  * Discovered a bug lurking within the scripts, or a papercut that bothers you
    just enough to make you want to actually do something about it? You guessed
    it: fork crouton, fix everything, and create a pull request.
  * Are most bugs too high-level for you to defeat? Grind up some
    [EXP](https://en.wikipedia.org/wiki/Experience_point) by using
    your fork to eat [pie](https://github.com/runeandre/crouton_for_the_oldbooks/labels/pie).


## Are there other, non-Contributory ways I can help?

Yes!


## But how?

There's a way For Everyone to help!

  * Something broken? File a bug! Bonus points if you try to fix it. It helps if
    you provide the output of `croutonversion` (or the output of
    `cat /etc/lsb-release` from Crosh) when you submit the bug.
  * Look through [open issues](https://github.com/runeandre/crouton_for_the_oldbooks/issues?state=open)
    and see if there's a topic or application you happen to have experience
    with. And then, preferably, share that experience with others.
  * Find issues that need [wiki entries](https://github.com/runeandre/crouton_for_the_oldbooks/issues?labels=needswiki&state=open,closed)
    and add the relevant info to the [wiki](https://github.com/runeandre/crouton_for_the_oldbooks/wiki).
    Or just add things to/improve things in the wiki in general, but do try to
    keep it relevant and organized.
  * Really like a certain desktop environment, but not up for coding? Open or
    comment on a bug with steps to get things working well.


## License

crouton (including this eloquently-written README) is copyright &copy; 2016 The
crouton Authors. All rights reserved. Use of the source code included here is
governed by a BSD-style license that can be found in the LICENSE file in the
source tree.
