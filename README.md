# Simple Synapse Admin

This is a simple, single-page admin tool for Synapse that allows you to:
- List users
- Reset a user's password
- Deactivate a user
- List rooms
- List local room members
- Delete rooms
- List registration tokens
- Delete registration tokens
- Create new registration tokens

## How to use

First, [set up an admin user and get the access token](https://matrix-org.github.io/synapse/latest/usage/administration/admin_api/index.html#authenticate-as-a-server-admin). Then, enable access to your server's [administration endpoints](https://matrix-org.github.io/synapse/latest/reverse_proxy.html#synapse-administration-endpoints).

Open [Simple Synapse Admin hosted on GitHub Pages](https://yaky-dev.github.io/simple-synapse-admin/) or [Simple Synapse Admin hosted on yaky.dev](https://yaky.dev/apps/simple-synapse-admin/) and enter your homeserver address and admin access token.

"Save credentials" is an optional convenience feature. If saved, credentials will be automatically set next time you open the page. Credentials are saved to local storage in your browser and are not transmitted anywhere. "Clear credentials" clears credential inputs and all of local storage.

### Users

Users are displayed 20 at a time. Click "Next" to display the next batch or click "List users" to display the first page.

### Rooms

Rooms are displayed 20 at a time. Click "Prev" and "Next" to display the previous or next batch, accordingly. Click "List rooms" to display the first page.

Enter text into "Search" before clicking "List rooms" to show only rooms with matching names.
 
"Members" displays _local_ room members.

"Delete" attempts to delete a room. If the initial attempt fails, you can try to force purge the room, which might cause issues with users still in the room.

### Registration Tokens

Registration tokens are a way to control who can register new accounts on your server, useful to allow trusted people, but not the general public. See [documentation](https://matrix-org.github.io/synapse/latest/usage/administration/admin_api/registration_tokens.html).

When creating a new token, all fields are optional. By default, the new token will be a random 16-character string, with unlimited uses and no expiration.

## Known issues

API for users and rooms is quite different, so users can only be paginated forward, while rooms can be paginated forward and back.

When resetting a user's password, the default browser prompt displays text in the clear, so one might need to be careful.

Synapse cannot delete users. Deactivating a user with erase flag [deletes lots of info](https://matrix-org.github.io/synapse/latest/admin_api/user_admin_api.html#deactivate-account) but retains the user entry in the database, and the user remains visible through the API. Deactivate and erase claims to be GDPR-compliant, but the username can contain PII such as a name.

Some rooms may need to be force purged even if there are no local users in them.

Local storage has [undefined behavior when running locally](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage#description). It appears to work fine in Firefox, though.

## Source and privacy

Simple Synapse Admin is a single HTML page, less than 500 lines of uncompressed and readable code. It runs in the browser and only communicates to the Admin API of the specified homeserver, using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch). Credentials can be optionally saved to local storage, which resides on your machine.

## Running locally

You don't trust some guy named Yaky with your homeserver and access token, I get it. Or you don't expose your homeserver's API to the web, because you are a careful person. Just download the [page](https://raw.githubusercontent.com/yaky-dev/synapse-admin/main/index.html) and run it from your machine.

## Why though?

My Synapse server is small, but after joining and leaving several large rooms, using bridges, and converting my database from SQLite3 to Postgresql, there were some broken and orphaned rooms. Some of them just appear in my Element client, but I cannot leave them. Some simply exist without users and have not been cleaned up for some reason. Occasionally, I need to reset a password for a user. There were some forgotten test users that I want to deactivate or to restore access to. Also, I intend to invite some people to my Matrix server, but don't want to open registration to the public, so I needed a tool to handle registration tokens.
