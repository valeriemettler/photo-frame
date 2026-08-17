# iPad Photo Frame

Turns an iPad into a photo frame. One file, no apps to install, no monthly cost.
Photos come from a Dropbox folder, so anything you drop in that folder shows up
on the frame by itself.

The whole thing is `index.html`. That single file is the program.

---

## Part 1. Put the file somewhere Safari can open it

> **Already done.** The repository exists and the page is live at
> **<https://valeriemettler.github.io/photo-frame/>**
>
> Open that on the iPad and skip to Part 2. The rest of Part 1 is kept as a
> record of how it was set up, and in case you ever need to redo it.

**You cannot just copy the file onto the iPad.** Two reasons, both worth knowing
so the error messages make sense later:

1. Safari on iPad will not let you "Add to Home Screen" a file that lives on the
   iPad itself. Only real web addresses get that option.
2. The Dropbox sign-in needs a secure (`https://`) address. Apple blocks the
   security features it uses on anything else.

So the file needs to live at a web address. Here is the free way to do that.

### Putting it on GitHub Pages (about five minutes, free, always on)

1. Go to <https://github.com/new> and sign in.
2. **Repository name:** `photo-frame`
3. Choose **Public**. (This matters - the free web hosting only works on public
   ones. Nothing private is in the file. See "Is this safe?" below.)
4. Click **Create repository**.
5. On the new page, click **Add file** > **Upload files**.
6. Drag `index.html` into the box. Then click **Commit changes**.
7. Click the **Settings** tab at the top, then **Pages** in the left sidebar.
8. Under "Build and deployment", set **Source** to *Deploy from a branch*, set
   **Branch** to `main` and the folder to `/ (root)`. Click **Save**.
9. Wait about a minute, then reload that Settings > Pages page. It will show
   your address near the top.

Your frame is now at:

```
https://YOUR-GITHUB-USERNAME.github.io/photo-frame/
```

The file is named `index.html`, which is why the address has no file name on
the end - much easier to type on an iPad.

### Is this safe to put on a public page?

Yes. The page contains no photos and no passwords. The only Dropbox thing in it
is the "App key", which Dropbox designs to be public - on its own it opens
nothing. The actual permission to read your Dropbox is created when *you* tap
Approve on the iPad, and it is stored on that iPad only. Nobody loading the page
sees your photos; they just see an empty frame asking to be set up.

### Quicker way to try it first, before doing any of the above

On your Mac, in Terminal:

```
python3 -m http.server 8099 --directory ~/projects/ipad-photo-frame
```

Then open `http://YOUR-MACS-IP:8099/index.html` on the iPad, on the same
wifi. Good for checking the slideshow looks right using pasted photo links.
The Dropbox sign-in will **not** work this way, because it is not `https://`.

---

## Part 2. Add it to the Home Screen so it runs fullscreen

On the iPad:

1. Open the address in **Safari** (it has to be Safari, not Chrome).
2. Tap the **Share** button - the square with an arrow pointing up, at the top
   of the screen.
3. Scroll down the list and tap **Add to Home Screen**.
4. Name it "Photos" or whatever you like. Tap **Add**.
5. Close Safari. Open the new icon from the Home Screen.

It now runs fullscreen with no address bar and no buttons. That is the frame.

---

## Part 3. iPad settings you also need to change

The page asks iPadOS to keep the screen on, but a few settings can override it.

| Setting | Where | Set it to |
|---|---|---|
| **Auto-Lock** | Settings > Display & Brightness > Auto-Lock | **Never** |
| **Low Power Mode** | Settings > Battery | **Off** (it cancels keep-awake) |
| **Power** | - | Keep the iPad plugged in |

Optional but useful:

- **Guided Access** locks the iPad into the frame so a stray tap cannot leave
  it. Settings > Accessibility > Guided Access > On. Then open the frame and
  triple-click the Home button to start it. Triple-click and enter your passcode
  to get out.
- **Night Shift / True Tone** in Settings > Display & Brightness make the photos
  warmer in the evening. Personal preference.
- **Do Not Disturb** stops notification banners appearing over your photos.

---

## Part 4. Connecting Dropbox

This is a one-time setup. It takes about five minutes. You are creating a free
"app" inside your own Dropbox account, which is just Dropbox's way of saying
"this thing has permission to read one folder".

### On a computer: create the Dropbox app

1. Go to <https://www.dropbox.com/developers/apps> and sign in.
2. Click **Create app**.
3. **Choose an API:** pick **Scoped access**. (It is the only option.)
4. **Choose the type of access:** pick **App folder**.
   This is the safe choice - it means this frame can only ever see one folder,
   never the rest of your Dropbox.
5. **Name your app:** something unique, like `Valerie Photo Frame`. If it says
   the name is taken, add a number.
6. Click **Create app**.

### Give it permission to read photos

7. On the app's page, click the **Permissions** tab.
8. Tick these two boxes:
   - `files.metadata.read`
   - `files.content.read`
9. Click **Submit** at the bottom. **Do not skip this.** If you skip it, the
   sign-in appears to work but no photos ever load.

### Copy the App key

10. Click back to the **Settings** tab.
11. Find **App key** near the top. Copy it. It looks like `a1b2c3d4e5f6g7h`.

### On the iPad: connect

12. Open the frame from the Home Screen. It will say Dropbox is not connected.
13. Tap **Open Setup**.
14. Paste the App key into the first box.
15. Tap **Connect to Dropbox**. Dropbox opens and asks you to allow it.
16. Tap **Allow**. Dropbox then shows you a code - a long line of letters and
    numbers. Press and hold it, then **Copy**.
17. Go back to the frame (swipe up, or tap the Home Screen icon again).
18. Paste the code into the box that says "paste the code from Dropbox here".
19. Tap **Finish connecting**. It should say "Connected."
20. Tap **Save and start slideshow**.

That is it. The frame will not ask again - it renews its own access from now on.

**If the code is refused:** codes only work once. Tap **Connect to Dropbox**
again to get a fresh one, and use that.

---

## Part 5. Adding and changing photos

Open Dropbox on any device and go to:

```
Dropbox > Apps > [your app name]
```

Drop photos in there. That is the whole workflow.

- New photos appear on the frame within **10 minutes**, with no restart. To see
  them immediately, tap the screen, tap **Setup**, then **Save and start
  slideshow**.
- Sub-folders are included, so you can organise however you like.
- Delete a photo from the folder and it disappears from the frame.
- iPhone `.heic` photos work. The frame quietly asks Dropbox for a normal
  version of those.
- Works with: jpg, jpeg, png, gif, webp, heic, heif, avif, bmp, tif, tiff.
- Video clips play too: mp4, mov, m4v. See "Videos" below.
- Anything else in the folder is ignored.

### Showing where each photo was taken

Put a file ending `-photo-locations.csv` anywhere in your Dropbox folder and
the frame will label each photo with the place it was taken:

```
L'Antica Pizzeria da Michele
Napoli · Italia · September 2025
```

The frame does no location lookups of its own. It reads that spreadsheet once
at startup and matches rows to photos by file name, so there is no account,
no key, and nothing to pay. Photos with no row simply show the folder name, so
it is always safe to leave switched on.

Generate one for a folder with:

```
python3 ~/projects/photo-locations/build_locations_csv.py ~/Documents/YourFolder
```

Then upload the resulting `.csv` into the Dropbox folder alongside the photos.
Choose what the corner shows under **Label in the corner** in Setup.

### Opening the map for a photo

When a photo has a location, an **Open in Maps** button appears in the tap bar
next to Hide. It opens that spot in a new Safari tab - the business listing if
the place has a name, a pin on the exact coordinates if it does not.

It is a real link rather than a button, because iPadOS blocks scripted
window-opening from a Home Screen app. Photos with no location simply do not
show the button.

### Showing only videos

Setup has a **Show** row: Everything, Photos only, or Videos only. Each carries
a count for whatever folder you have selected, so you can see there are eight
clips before choosing to watch only those.

Asking for videos in a folder that has none shows that folder's photos rather
than an empty frame.

### Favorites

Tap the screen, then **☆ Favorite**, to star the photo or clip on screen (tap
again to unstar). Setup has a **Favorites** row - **Everything** or **Favorites
only**, with a count for the folder and Show choice you have selected. Asking
for favorites where there are none in that folder shows favorites from every
folder; if nothing is starred at all, the filter gives way rather than leave a
blank frame.

Favorites are kept in Dropbox as `_notes/favorites.txt` - one photo per line,
folder/name without the extension - so they survive a reset and reach any other
iPad using the same folder. The change is made on the iPad at once and written
up a moment later; if Dropbox cannot be reached right then, it goes up on the
next chance. Edit the file in Dropbox and the frame picks it up on its next
check. Photos from pasted links can be starred too, but those stay on the iPad.

### How photos change

Setup has a **How photos change** row. Tap one and the next photo uses it.

| Choice | What it looks like |
|---|---|
| **Dissolve** | One photo melts into the next. The classic, and the default |
| **Through black** | Fades down to black, then up into the next. Unhurried, cinematic |
| **Slide across** | The next photo slides in from the right |
| **Slow push in** | Dissolve with a gentle zoom, like a camera settling |
| **Blur through** | The old photo blurs away as the new one comes into focus |
| **Surprise me** | A different one each time |

`FADE_SECONDS` controls how long any of them take. If the iPad has Reduce
Motion switched on in Accessibility settings, everything falls back to a plain
dissolve.

### How long each photo stays

Setup has a **How long each photo stays** row: half a second up to 5 minutes.
Tap one and it counts from the next photo. Video clips ignore it and play all
the way through (up to `MAX_VIDEO_SECONDS`, 60 by default). The file's own
`SECONDS_PER_PHOTO` value is always one of the pills, so you can get back to it.
On the very short settings the cross-fade shortens to fit (half the dwell time),
so photos still change cleanly.

### Voice notes

Tap the screen, then **Record note**, and talk about the photo on screen. Tap
**Stop and save**. The recording goes into your Dropbox photo folder under
`_notes/`, mirroring the photo's own sub-folder and named after it plus the
time you recorded - for example
`_notes/Trips/IMG_4021.20260817-103200.m4a`. Several notes per photo are fine.

While you talk, the iPad's own dictation types out what you say (the words
appear in the recorder card) and that text is saved beside the recording as a
`.txt` file. If dictation is not available - it needs Siri and Dictation
switched on, and Apple only sometimes allows it inside Home Screen apps - the
note still saves as a recording. Setup has a **Voice notes** row where you can
turn dictation off (**Voice only**), and a status line saying what this iPad
can do and how many notes were found.

Photos with a note get a small **Voice note** tag in the corner opposite the
label. Tap it, or tap **Info** in the control bar, for the Info card: where and
when the photo was taken, each note with its transcript and a play button,
**Record a note**, and **Open in Safari**. That last one opens the same
information as a plain page in Safari. It carries everything it needs in its
web address (a Safari tab cannot see this app's Dropbox connection - Safari
and Home Screen apps keep separate storage), using temporary Dropbox links
that last about four hours; open it again from Info for fresh ones.

Dictation gets words wrong. In the Info card each note has an **Edit** button:
fix the text and tap **Save**, and the `.txt` in Dropbox is replaced. A voice-only
note shows **Add text** instead. Saving an empty box removes the transcript file
and the note goes back to voice only. You can also edit the `.txt` files
straight in Dropbox from any device - the frame notices the change on its next
check and re-reads them. (The Safari page is read-only.)

The slideshow stands still while you record or read, and carries on when you
finish. Recording stops by itself after `MAX_NOTE_SECONDS` (three minutes).

Two one-time things:

- The Dropbox app needs the **files.content.write** permission ticked in the
  App Console, and then the frame has to be connected again once (Setup >
  Disconnect > Connect) so its permission includes writing. Until then, saving
  shows "Dropbox has to be reconnected once" and keeps your recording ready
  for **Try again**.
- The iPad will ask to allow the microphone the first time. If it never asks,
  Settings > Safari > Microphone (or the entry for this app) > Allow.

### Quiet hours

Setup has a **Quiet hours** section. The frame fades to black at your chosen
time and comes back on its own in the morning. Tap the dark screen to peek at
the photos for five minutes.

Windows that cross midnight (23:00 to 07:00) work fine. Set both times the same
to switch it off.

### Hiding a photo, and getting it back

Tap the screen while it is showing, then tap **Hide this**. It disappears from
the rotation immediately. The photo stays in Dropbox untouched - the frame just
skips it.

Three ways back:

- **Undo hide** appears next to it for 30 seconds after you hide something.
- **Record note** starts a voice note for the photo on screen; **Info** shows what is known about it and its notes (see Voice notes below).
  For the times you meant to tap Next.
- **Review hidden photos**, in Setup, turns the rotation inside out and shows
  only the hidden ones. An orange banner across the top makes it obvious, and
  **Hide this** becomes **Unhide this**. Tap **Done** when finished. Unhiding
  the last one drops you out automatically.
- **Unhide all of them**, in Setup, restores everything at once.

### Videos

Video clips in the folder play automatically, muted, then the frame moves on.
`mp4`, `mov` and `m4v`. A clip plays for its own length, up to
`MAX_VIDEO_SECONDS`.

If you upload iPhone Live Photos you get both a still and a two-second clip.
Set `SKIP_CLIPS_SHORTER_THAN_SECONDS` to `3` to skip those stubs, or leave it
at `0` to play everything.

### Choosing a folder and an order, from the iPad

Tap the screen, tap **Setup**, and the first two things are:

- **Photos from** - every folder inside your Dropbox folder, plus **All
  folders** to mix them together. The number on each is how many photos it has.
- **Favorites** - Everything, or Favorites only (see Favorites above).
- **Order** - Shuffle, Newest first, or Oldest first.

Tap what you want, then **Show these photos**. The choice is remembered on that
iPad, so different iPads can show different folders.

Folders only appear here once they have photos in them. Photos sitting loose in
the main folder are still shown under "All folders" - they just have no folder
name to list.

**The corner label** shows which folder each photo came from. Photos sitting
loose in the main folder show nothing, since they are not in a folder. Change
`CORNER_SHOWS` to `"clock"` to get the time and date back instead, or
`"folder+clock"` for both.

**Photo shape:** the iPad's screen is 4:3. Photos are scaled to fit without ever
being cropped, so a wide photo gets black bars above and below. That is
deliberate - nothing gets its heads cut off.

---

## Part 6. Settings you can change in the file

Open `index.html` in TextEdit (or any text editor). Everything adjustable
is in one clearly marked block near the top. Change a value, save, re-upload to
GitHub, then reload the page on the iPad.

| Setting | What it does | Default |
|---|---|---|
| `SECONDS_PER_PHOTO` | How long each photo stays up. Also settable on the iPad | `12` |
| `NOTES_FOLDER` | Sub-folder where voice notes and transcripts are kept | `"_notes"` |
| `DICTATION` | `true` = type out what you say while recording a note. Also settable on the iPad | `true` |
| `MAX_NOTE_SECONDS` | Longest single voice note | `180` |
| `FAVORITES_FILE` | Name of the favorites list inside the notes folder | `"favorites.txt"` |
| `SHUFFLE` | `true` = random order, `false` = newest first | `true` |
| `TRANSITION` | `"fade"`, `"dip"`, `"slide"`, `"zoom"`, `"blur"` or `"random"`. Also settable on the iPad | `"fade"` |
| `FADE_SECONDS` | Length of the cross-fade. `0` = instant | `1.5` |
| `CORNER_SHOWS` | What the corner label shows: `"folder"`, `"clock"`, `"folder+clock"`, `"filename"`, `"none"` | `"folder"` |
| `CORNER_POSITION` | `"top-left"`, `"top-right"`, `"bottom-left"`, `"bottom-right"` | `"bottom-right"` |
| `SHOW_DATE` | Date line under the time. Clock modes only | `true` |
| `CLOCK_24_HOUR` | `true` = 14:30, `false` = 2:30 PM | `false` |
| `NO_FOLDER_LABEL` | Label for photos not inside any sub-folder | `""` |
| `KEEP_SCREEN_AWAKE` | Ask iPadOS to keep the screen on | `true` |
| `CHECK_FOR_NEW_PHOTOS_MINUTES` | How often to look for new photos | `10` |
| `SLEEP_ENABLED` | Go black overnight. Also settable on the iPad | `true` |
| `SLEEP_START` / `SLEEP_END` | Quiet hours, 24-hour `"HH:MM"` | `"23:00"` / `"07:00"` |
| `SLEEP_PEEK_MINUTES` | How long a tap wakes it during quiet hours | `5` |
| `PLAY_VIDEOS` | Play video clips found in the folder | `true` |
| `MAX_VIDEO_SECONDS` | Longest a single clip holds the screen | `60` |
| `SKIP_CLIPS_SHORTER_THAN_SECONDS` | Skip Live Photo stubs. `0` plays everything | `0` |
| `USE_SMALLER_PHOTOS` | Download 2048-pixel copies instead of full-size originals. Looks identical on an iPad, uses far less data | `true` |
| `DROPBOX_FOLDER_PATH` | Leave empty for the app's own folder | `""` |

When you edit, keep the quotation marks and the semicolon exactly as they are.
`true` and `false` are lowercase and never in quotes.

---

## Part 7. The backup option - just paste photo links

If Dropbox turns out to be more trouble than it is worth, you can skip it
entirely and hand the frame a list of photo addresses.

**Easiest way:** on the iPad, tap the screen, tap **Setup**, scroll to
"Backup option: paste photo links", and paste one web address per line. Tap
**Save and start slideshow**.

**Or in the file:** find `const PHOTO_URLS = [` near the top and fill it in:

```javascript
const PHOTO_URLS = [
  "https://www.dropbox.com/scl/fi/abc123/beach.jpg?rlkey=xyz&dl=0",
  "https://www.dropbox.com/scl/fi/def456/garden.jpg?rlkey=xyz&dl=0",
];
```

Each address in quotes, each followed by a comma.

To get a link for one photo: in Dropbox, hover the photo, click **Share**, then
**Copy link**. Paste it straight in. The ending Dropbox gives you (`?dl=0`)
shows its own web page rather than the picture, so the frame corrects that
automatically. Google Drive share links get corrected too.

The catch: this list is fixed. Adding a photo to Dropbox does nothing - you have
to add its link here by hand. That is exactly what the full Dropbox setup avoids.

---

## Part 8. Google Photos - the honest answer

**Short version: Google Photos cannot drive this frame, and no amount of code
fixes that.**

Until 31 March 2025, an app could ask permission to read your Google Photos
library or a named album and re-check it on a loop, which is what a photo frame
needs. Google removed those permissions. Any app asking for them now gets
refused with a "403" error. There is no workaround, no paid tier that restores
it, and no sign of it coming back.

What Google left behind is the **Picker API**. With it, an app can only show you
Google's own picker and receive whatever photos *you* tap, one session at a time.
For a photo frame that means:

- You would hand-pick the photos every single time. There is no "watch this
  album" - adding a photo in Google Photos would do nothing.
- The picking session expires after about a day, and the photo links expire
  after about an hour, so the frame has to keep re-fetching.
- It needs a Google Cloud project, a consent screen, and you listed as a test
  user. If you stay on the test-user route, the sign-in lapses every 7 days and
  you have to redo it.
- Photos cannot be shown directly. Each one has to be downloaded with your
  sign-in attached first, then displayed.

That is more setup than the entire Dropbox path above, for something that stops
working every week and never picks up new photos on its own. **So it is not
built here.** There is a long comment block at the bottom of `index.html`
recording exactly what it would involve and which web addresses it would use, in
case you ever want it.

**What to do instead:** if the photos you want are in Google Photos, download
them once and drop them in the Dropbox folder. From then on they behave like
every other photo in the frame.

---

## Part 9. Using the frame

- **Tap the screen** - a small bar appears with Back, Pause, Next and Setup.
  It fades away after five seconds.
- **Tap again** - hides the bar.
- If you attach a keyboard: left and right arrows change photo, space pauses.

---

## Part 10. If something goes wrong

The frame never shows a blank black screen. If something is wrong it says so on
screen, with the technical detail in small grey text at the bottom.

| What you see | What it means | Fix |
|---|---|---|
| "Dropbox is not connected yet" | Setup not done, or done in Safari instead of the Home Screen app | Tap Open Setup and do Part 4 steps 12-20, **inside the Home Screen app** |
| "Dropbox needs reconnecting" | The saved permission stopped working | Tap Open Setup, tap Connect to Dropbox, redo steps 15-20 |
| "That Dropbox folder is empty" | The folder has no photos in it yet | Add photos to Dropbox > Apps > [your app name] |
| "That Dropbox folder was not found" | The folder path is wrong | In Setup, clear the folder box completely |
| "Could not reach Dropbox" | No internet | Check wifi. It retries every minute by itself |
| Dropbox connects but no photos load | The Permissions step was skipped | Do Part 4 steps 7-9, then reconnect on the iPad |
| Screen still goes to sleep | Auto-Lock or Low Power Mode | See Part 3 |
| Setup screen has forgotten everything | Safari and the Home Screen app keep separate memory | Do the Dropbox setup inside the Home Screen app, not in Safari |

**One thing worth repeating:** do the Dropbox setup *inside the Home Screen
app*, not in Safari. iPadOS gives them separate storage, so connecting in one
does not connect the other.

---

## Files here

- `index.html` - the frame. This one file is the whole program.
- `README.md` - this document.
