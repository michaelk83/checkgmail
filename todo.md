# To Do
## Original
Post v1.10 ...

- Add ability to change the address used on left clicking the icon
- Icon preview frame in filechooser dialogue?

## New
**UPDATE 2021-05-09:** These will probably never get done, since I prefer to put the effort in a modern notifier based on Qt and the current GMail API. I've renamed my `v1.15` branch to a more generic `develop` to finally upload it to GitHub (I've been planning to upload it for a long time, but never got around to it).

#### v1.15: Cosmetic Fixes
* ~~apply previous patches~~
    * ~~bug: numbers are not centered correctly~~
    * ~~make numbers on by default and/or add a setting for it.~~
    * ~~other fixes~~
* ~~fix icons~~
* (maybe fix) two-factor bug: the PIN gets sent every time the app logs in, even if there's already a trust cookie saved.
* (maybe fix) use the main email/password dialog to get the main password - see bellow for details.
    * don't forget to update the comments near line ~~590~~ (line numbers may need update) and maybe elsewhere.
* possible bug: the tray icon background color isn't set until after the login credentials are requested.
* bug: unread count greater than 20 still shows as 20 in the tray icon.
    * It's fine to only show the first 20 messages in the popup list, but the tray should show the full count.
    * This may be a limitation of the communications protocol.
* rename `cookie_login` option to `cookies`
* update `-h` output
* unzip manpage for better version tracking
* update manpage file
* ~~maybe rename `Readme` to `README.md`,~~ and update it.
* bump version to 1.15.0 and update changelog


#### v1.16: Auth Refactoring
* (partially done) make `two_factor` the default when `no_cookies` isn't specified, otherwise login isn't possible (an app password doesn't work with cookies: cookie is saved, but still getting "Failed login or unable to find gmail_ik" and "Error : 401 Unauthorized").
    * (partially done) or better yet, `no_login`, if neither is specified.
    * ~~CANCELED for the following reasons:~~
        1. Two-factor authentication may be disabled in gmail, in which case cookie auth with the regular web password may still work (untested).
        2. Stopping login if the user clicks Cancel prevents the user from quiting the app, since it just tries to log in again, and during this time the context menu is inaccessible. Fixing this will require larger changes to the main loop, which are not worthwhile given that:
        3. The user can still access the preferences UI while the login dialog is open and waiting for user input.
* two-factor bug: if an app-specific password is not saved, it still asks for it even if both two-factor auth and cookies are enabled.
    * to fix, first use the main email/password dialog to get the main password. decide which by the usual criteria (two-factor auth and cookies are both enabled), and/or by an additional parameter passed from `get_web_passwd()`.
        * IMPORTANT: double-check that you don't need an app-specific password for anything if two-password auth is used. E.g. see around line ~~1000-1005~~ (line numbers may need update)
            * lines 1000-1005 suggest that it needs an app-specific password for the atom feed, but it seems to be working find without one, at least if a trust cookie is set.
            * if we don't need an app-specific password then maybe we don't need the distinction at all, and can just use a single password, which will be different depending on the case?
                * We should probably still keep separate passwords in kdewallet, but we only need to keep one password in memory.
                * => POSTPONE to version 1.16, for a larger refactoring and fix-up of the password handling and two-factor authentication. (Or CANCEL for lack of interest.)
        * the main dialog may still need to be called from `get_web_passwd()` for the password to be handled correctly.
        * the overall fixed flow should be:
            A. if not using two-factor auth, login with a single password as usual (be that the app password or normal web password).
            B. otherwise:
                1. first, try to load the saved trust cookie silently, if available.
                2. ask for the email and main password. if the trust cookie is available, set it to avoid the verification code from being sent (maybe - not sure if that's how it works).
                3. if a trust cookie is not available, submit the main password to get the verification code sent, then ask for it from the user.
                    * don't ask for the verification code in advance! only ask for it when it's actually needed during the login process.
                4. during the periodic check, if there's a problem
* two-factor bug: when main password is entered, it's not submitted immediately, so no PIN code is sent.
* two-factor bug: if PIN code is not entered and OK is clicked (or if Cancel is clicked), normal login dialog is shown instead of the PIN dialog.
* two factor bug: if either of the dialogs is canceled, the application doesn't exit.


