Welcome to PawPrint

Features
- Dog walk paths
  - Record walked paths (raw GPS data)
  - Record metadata
    - distance, duration, pace
    - start/end time
    - elevation
    - which dogs were walked
    - photos, notes
  - Storage: PostgreSQL + PostGIS for metadata/routes, raw GPS tracks in S3 - see docs/walks-and-gps.md
  - Visibility set per walk (e.g. private, followers, public)

- Account system
  - Profiles
    - username, bio, profile pics
    - display walked paths on profile (respecting each walk's visibility)
  - Dogs
    - own records, can be linked to multiple owners
    - name, breed, age, temperament, pic
  - Following/Followers
    - feed of walks from people you follow, in time order
  - Friends
    - chat
    - share walks

Deferred
- Groups - extension of friends, build once friends are implemented
- Ability to advertise "job" to walk dog (borrow my doggy)

Notes / to address later
- Privacy: user-defined privacy zones clipped from public walks so home addresses aren't exposed - see docs/walks-and-gps.md
