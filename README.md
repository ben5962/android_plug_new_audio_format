# android_plug_new_audio_format
#  what? 
just a student toy project to try to add a custom audio format to the android audio stack
#  just why? 
Object oriented programming should enable anyone to plug some new feature without having to read a whole project sourcecode.
I want to verify this assertion by doing.
What is the minimal information necessary to do such a task?

# frontal approach 
class diagrams generated from a mediaplayer or an exoplayer git sourcecode.
conclusion:  hard to find a way to reduce volume of information.

# mediaplayer or exoplayer documentation
the android audio stack is considered understood in this documentation.
I just found details about specific classes of both projects. 
pcm audio format (independantly of his caracteristics) seems to play a central role
for soundcards, because it is the stream format outputted by android codecs. 
conclusion: I have to understand android audio stack

# android audio stack documentation 
![la pile audio android](https://source.android.com/devices/audio/images/ape_fwk_audio.png) 

quickly looking at the documentation and reading some sourcecode does not bring me much information
about where the new lib should be put, and how i could plug it to the android high level interface, 
though it has to be connnected to jni in some way, to cross the langage barrier.
of course, i could just stackoverflow it, but that's not the rule of the game:
do it with minimal documentation, basic computer knowledge and aosp, exoplayer and mediaplayer sourcecode tree. 


# back to basics
I may find the place where soundformats are defined in android by defining the steps of sound processing.
- sound is produced by vibration of air
- vibration of air is produced by speaker, moved by electrical signal
- electrical signal is produced by soundcard
- representation of soundcard is pcm. soundcard receives pcm stubs by reading some memory shared between soundcard and computer.

I need containers, wav for exemple, because
even if I  have to feed the soundcard with pcm, 
its caracterised (sampling depth and frequency) 
are not carried with pcm format.
furthermore, I can put several pcm channels within a container, or 
any other content that represents one channel of sound.

codec has to:
- open the container
- extract information about inner content (format, sampling rate, sampling frequency, ...)
- extract content
- convert content to pcm

then I have to feed dma buffer with pcm somehow, at the right pace. 
