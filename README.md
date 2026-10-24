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
  - Storage: Strava reportedly stores raw GPS data in S3 with NoSQL DBs for metadata - worth looking into
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
- Privacy: blur or trim start/end points of public walks so home addresses aren't exposed
