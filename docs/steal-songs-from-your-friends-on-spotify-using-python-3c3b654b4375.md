# 使用 Python 从 Spotify 上的朋友那里窃取歌曲

> 原文：<https://medium.com/analytics-vidhya/steal-songs-from-your-friends-on-spotify-using-python-3c3b654b4375?source=collection_archive---------16----------------------->

![](img/096437d9d22231458b2994e09408cfe8.png)

你有没有想过如何创建一个朋友在 Spotify 上听的播放列表？Spotify API 和 Spotitpy python 库使这变得简单易行。

首先，您需要熟悉使用 python 中的 Spotipy 库。如果你以前使用过这个工具，请继续阅读，否则，我创建了这篇文章，涵盖如何开始的基础知识:[https://maxpiersanti . medium . com/automating-Spotify-with-python-everything-you-need-to-know-the-spot ipy-library-f 84d 60 E8 a3 EC](https://maxpiersanti.medium.com/automating-spotify-with-python-everything-you-need-to-know-about-the-spotipy-library-f84d60e8a3ec)

这个项目的设置相当简单。首先，我们将创建一个助手类，以便使一些设置过程更加简化。

```
import spotipy
import spotipy.util as util 
from spotipy.oauth2 import SpotifyClientCredentials
from spotipy.oauth2 import SpotifyOAuthclass Setup(object):
    cid = ''
    secret = ''
    primaryUsername = ''
    scope = "playlist-modify-public"def __init__(self, cid, secret, primaryUsername):
        Setup.cid = cid
        Setup.secret = secret
        Setup.primaryUsername = primaryUsernamedef createAndReturnAuthenticatedSpotifyInstance(self):
        token =       util.prompt_for_user_token(Setup.primaryUsername,Setup.scope,client_id=Setup.cid,client_secret=Setup.secret,redirect_uri='[http://localhost/'](http://localhost/')) 
        spotify = spotipy.Spotify(auth=token)
        return spotifydef initializeData(self, friendUsernames, spotify):
        totalTracks = []
        friendTrackData = []
        for i in range(len(friendUsernames)):    
            username = friendUsernames[i]
            playlists = spotify.user_playlists(username)
            userTracks = []
            while playlists:
                for i, playlist in enumerate(playlists['items']):           
                    playlistTracks =                       spotify.playlist_tracks(playlist['uri'])
                    trackItems = playlistTracks['items']    
                    for k in range(len(trackItems)):
                        trackItem = trackItems[k]
                        track = trackItem['track']
                        userTracks.append(track['uri'])
                        totalTracks.append(track['uri'])
                if playlists['next']:
                    playlists = spotify.next(playlists)
                else:
                    playlists = None
            dictOfData = {"user":username, "userTracks":userTracks}
            friendTrackData.append(dictOfData)
        return totalTracks, friendTrackData
```

因此，在这个类中，我们初始化了 Spotipy 实例，获得了从本地主机 promt 读取和写入播放列表的用户权限，并创建了一个 helper 方法，该方法将收集我们自己的个人播放列表和公共播放列表中的所有曲目，我们稍后将填充这些曲目。

现在创建一个新的 python 文件，作为我们应用程序的主要驱动程序。

```
import spotipy
import spotipy.util as util
from spotipy.oauth2 import SpotifyClientCredentials
from spotipy.oauth2 import SpotifyOAuth
import SetupModule
import math
from collections import Countercid = 'your cid'
secret = 'your secret'
username = 'username of user'
friendUsernames = [#add in friend usernames in this format                         'username' + , #]
playlistLength = 100
maxMatchFactor = len(friendUsernames)
numFriends = len(friendUsernames)
compilerId = 'spotify:playlist:identifier for playlist to fill'
setupTool = SetupModule.Setup(cid,secret, username)
spotify = setupTool.createAndReturnAuthenticatedSpotifyInstance()# find the most tracks that each individual liked the most
def findIndividualFavorites():
    allFavorites = []
    personalShare = math.ceil((0.5 * playlistLength)/numFriends)
    for i in range(len(friendTrackData)):
        curUser = friendTrackData[i]
        tracks = curUser['userTracks']
        occurence_count = Counter(tracks) 
        mostPopular = occurence_count.most_common(personalShare)
        temp = dict(mostPopular)
        allFavorites += temp.keys()        
    return allFavorites# add track operation can only support a list of max size 100 so if we have more track than that 
# break total tracks into a bunch of size 99 chunks and add those iteratively
def addTracksToPlaylist(tracksToAdd):  
    if len(tracksToAdd) > 99:
        generator = chunks(tracksToAdd,99)
        for value in generator:  
            spotify.user_playlist_add_tracks(username, compilerId,               value, position=None)
    else:
        spotify.user_playlist_add_tracks(username, compilerId,          tracksToAdd, position=None)# find songs that everyone likes def findPopularAmongAll(matchFactors):
    popularAmongAll = []
    while( len(popularAmongAll) != math.ceil((0.5*playlistLength))):
        for p in range( maxMatchFactor-1,0,-1):
            if  len(matchFactors[p]) != 0 :
                popularAmongAll.append(matchFactors[p].pop())
                break
    return popularAmongAlldef sortByMatchFactor(totalTracksClean):
    matchFactorCollection = {k: [] for k in range(maxMatchFactor)}

    for x in range(len(totalTracksClean)):
        curTrack = totalTracksClean[x]
        trackMatchFactor = getMatchFactor(curTrack)
        for i in range(maxMatchFactor,0,-1):
            if trackMatchFactor > i:
                matchFactorCollection[i].append(curTrack)
                break
    return matchFactorCollection# match factor is the number of friend's who had this track in their # playlistdef getMatchFactor(the_track):
    matchFactor = 0
    for i in range(len(friendTrackData)):
        curUser = friendTrackData[i]
        tracks = curUser['userTracks']
        if tracks.count(the_track) > 0:
            matchFactor += 1
    return matchFactordef chunks(lst, n):
    for i in range(0, len(lst), n):
        yield lst[i:i + n]playlists = spotify.user_playlists(username)
totalTracks, friendTrackData =  setupTool.initializeData(friendUsernames, spotify)
totalTracksClean = list(dict.fromkeys(totalTracks))
matchFactors = sortByMatchFactor(totalTracksClean)
popularAmongAll = findPopularAmongAll(matchFactors)
individualFavs = findIndividualFavorites()
tracksOfPlaylist = popularAmongAll + individualFavs
addTracksToPlaylist(tracksOfPlaylist)
```

首先，我们初始化开始与 Spotify API 通信所需的字段。填写顶部的字段后，我们使用 setupTool 从您选择的朋友的公共播放列表中获取所有曲目数据。

目前，我正在创建一个 100 首曲目的播放列表，其中 50 首曲目来自具有广泛吸引力的歌曲，50 首来自个人喜爱的歌曲，但这种算法可以根据需要轻松更改。

如果您有任何问题，请留下您的评论，我可以帮助您诊断任何问题。