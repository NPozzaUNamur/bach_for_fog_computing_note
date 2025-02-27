# Article
[[linda_in_the_fog(previous_thesis).pdf]]
# Summary
# Notes

## Server implementation

Implementation of network access of shared space [[linda_in_the_fog(previous_thesis).pdf#page=45&selection=27,68,28,25&color=yellow|p.35]]

Protocol of communication used is TCP/IP or UDP/IP (dual implementation) [[linda_in_the_fog(previous_thesis).pdf#page=45&selection=30,26,30,63&color=yellow|p.35]]

Repository is an collection of shared space with the permissions associated to each of them. [[linda_in_the_fog(previous_thesis).pdf#page=45&selection=37,56,39,19&color=yellow|p.35]]

Use concurrency management to handle concurrency errors [[linda_in_the_fog(previous_thesis).pdf#page=46&selection=83,42,84,86&color=yellow|p.36]]

Uses Arc to handle multiple access to the same value [[linda_in_the_fog(previous_thesis).pdf#page=46&selection=103,44,108,10&color=yellow|p.36]]

Uses Mutex to handle one access at a time to a shared space [[linda_in_the_fog(previous_thesis).pdf#page=47&selection=0,84,1,60&color=yellow|p.37]]

Uses RwLock to handle multiple access to list of sharespace, but lock if multiple writing (only happens when adding or deleting sharespace not modifying it) [[linda_in_the_fog(previous_thesis).pdf#page=47&selection=22,0,25,0&color=yellow|p.37]]

Requirement for access control system on IoT devices [[linda_in_the_fog(previous_thesis).pdf#page=48&selection=30,80,31,37&color=yellow|p.38]] + Existing system analysis [[linda_in_the_fog(previous_thesis).pdf#page=49&selection=12,64,13,26&color=yellow|p.39]]

Uses ABAC access control system [[linda_in_the_fog(previous_thesis).pdf#page=50&selection=3,0,4,16&color=yellow|p.40]]




