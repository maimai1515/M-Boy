# M-Boy
// References
var VC;
var TC;
var AS;
var DS;
var playlist = new Array();
var reqlist = new Array();
var volume = 100;
var fulltitle;
function Next()
{
  if(!(DS===undefined)) DS.pause();

  if (playlist.length > 0)
  {
    // Delete the first audio file in the list (the one that just finished)
        var fileRemove = false
        try {
            fs.remove(playlist[0].file)
            console.log('File removed!');

            fileRemove = true
        } catch(e) {
            fileRemove = false
            console.err(e)
        }

        if (fileRemove = true) {
            playlist.splice(0, 1);
            console.log('Spliced!');
        } else {
            console.err('Could not splice or file not removed.')
        }

        //console.log(playlist)
        //fulltitle = playlist[0].info.fulltitle
        return StartPlay(!mreturn);
  }

    console.log('Length: ' + playlist.length)

    if(playlist.length === 0) {
        client.user.setPresence( { status: 'idle', game: { name: null } } )
    }// else {
    //   // Start playing
    //   return StartPlay(!mreturn);
    // }
}
function StartPlay(!m) // Automatically plays queue once a song has been added successfully
{
  if (playlist.length >= 1) {
    // Play the audio file
    DS = AS.playFile(playlist[0].file);
    DS.passes = 12;
    DS.setVolume(volume/200.0);
    //fulltitle = playlist[0].info.fulltitle

        // Display song title in 'Game played'
    client.user.setPresence({ status: 'online', game: { type:2, name: fulltitle } });

        // Once the song is done
    DS.on('end', reason => {
      // Go to the next track
      Next(!mnext);
    });
  }
}
case 'skip':
    if(VC===undefined) return;
    if(TC===undefined) return;
    if(!CheckTCVC(message)) return;
    if(playlist.length === 0) return message.channel.send('There is nothing to skip.');
    if(playlist.length === 1) return message.channel.send('There is nothing to skip to. Queue something else first or use `' + prefix + 'leave`.')

        if(message.content.length > 5) {
            const trackNumber = parseInt(message.content.split(' ')[1])

            if (trackNumber !== 1) {
                fs.remove(playlist[trackNumber-1].file).then(() => {
                    console.log('File removed!');
                    playlist.splice(0, trackNumber-1)
                    message.channel.send('Future song skipped!')
                }).catch(err => {
                    console.error(err)
                })
            } else {
                Next(!mskip);
                message.channel.send('Current song skipped!')
            }
        } else {
            Next(!mskip);
            message.channel.send('Song skipped!')
        }

        break;
