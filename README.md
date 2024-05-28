; Rainmeter Glados potato Public V 1.1.1
; Written by Reece Ram
; https://www.deviantart.com/izy2091/art/1056975307
;
; Raw file code below

[Rainmeter]
Update=1000

[Metadata]
Name=Glados P0T@To
Author=IZY2091
Version=1.1.0
License=Creative Commons Attribution-NonCommercial-ShareAlike 3.0. Built on top of Tomtom5893's (deviantart) original idea
Information=Hello I'M A P0T@To. 

; Portal 2 potato
; Set "StartTime" and "EndTime" to prevent welcome messages 
; If Special days are set they will trigger specail events
; JokeDay default to April 1st
; Anniversary/Birthday off by default
;
; Todo
; edit audio files to prevent cut off of start of some sounds

[Variables]
; Put in times to prevent welcome message during certain times of day 24 hour clock format
StartTime=	00
EndTime=	24

; Special Dates all in (MM.DD) Format
JokeDay= 		04.01
Anniversary= 		00.00
Birthday=		00.00

; Click Sounds
SoundClickNormal= 	"Poke\Poke ([NumberCalcPoke]).wav"
SoundClickAprilFools= 	"Special\AprilFools\fact[NumberCalcPokeApril].wav"
SoundClickPartyHorn= 	"Special\PartyHorn\PH[NumberCalcPartyHorn].wav"
SoundClick=		#SoundClickNormal#
; Special Images
PicApril=	"Special\AprilFools\Fact.png"
PicAnniv=	"Special\Anniversary\RS.png"
PicBDay=	"Special\Bday\BDay.png"

[MeasureTime]
;Measure Time
Measure=Time
Format=%H

[MeasureDay]
;Measure Date
Measure=Time
Format=%m.%d

[NumberCalcHi]
; number of Hi audio 0-53 no ned to update since this will run once per load
Measure=Calc
Formula=Random
HighBound=53

[NumberCalcPoke] 
;number of poke audio 0-92
Measure=Calc
Formula=Random
HighBound=92
UpdateRandom=1
UpdateDivider=-1
Group=GroupRandom

[NumberCalcPokeApril]
;number of April audio 0-64
Measure=Calc
Formula=Random
HighBound=64
UpdateRandom=1
UpdateDivider=-1
Group=GroupRandom

[NumberCalcPartyHorn] 
;number of poke audio 0-7
Measure=Calc
Formula=Random
HighBound=7
UpdateRandom=1
UpdateDivider=-1
Group=GroupRandom

[CurrentYear]
Measure=Time
Format=%Y
UpdateDivider=-1
[StampForDateAfterBirthday]
Measure=Time
Timestamp=#Birthday#.[CurrentYear]
TimeStampFormat=%m.%d.%Y
Disabled=1
UpdateDivider=-1
[DateAfterBirthday1]
Measure=Time
Format=%m.%d
Timestamp=( [StampForDateAfterBirthday:Timestamp] + 86400 * 1)
UpdateDivider=-1
[DateAfterBirthday2]
Measure=Time
Format=%m.%d
Timestamp=( [StampForDateAfterBirthday:Timestamp] + 86400 * 2 )
UpdateDivider=-1

[TheAudioThatWillPlay]
Measure=String
String=[#SoundClick]
DynamicVariables=1
Group=GroupRandom

[MeasureWelcomeSound]
; Welcome Audio
Measure=Calc
; No special day
IfCondition=(MeasureTime > #StartTime# ) && (MeasureTime < #EndTime#) && (MeasureDay <> #JokeDay#) && (MeasureDay <> #Anniversary#) && (MeasureDay <> #Birthday#) 
IfTrueAction=[PLAY \Hi\Hi ([NumberCalcHi]).wav]
;JokeDay
IfCondition5=(MeasureTime > #StartTime# ) && (MeasureTime < #EndTime#) && (MeasureDay = #JokeDay#)
IfTrueAction5=[PLAY \Special\AprilFools\fact[NumberCalcPokeApril].wav]
;Anniversary
IfCondition6=(MeasureTime > #StartTime# ) && (MeasureTime < #EndTime#) && (MeasureDay = #Anniversary#)
IfTrueAction6=[PLAY \Special\PartyHorn\PHLong.wav]
;Birthday
IfCondition2=(MeasureTime > #StartTime# ) && (MeasureTime < #EndTime#) && (MeasureDay = #Birthday#)
IfTrueAction2=[PLAY \Special\Bday\BDay.wav]
;Birthday+1
IfCondition3=(MeasureTime > #StartTime# ) && (MeasureTime < #EndTime#) &&(MeasureDay = DateAfterBirthday1) && (00.00 <> #Birthday#)
IfTrueAction3=[PLAY \Special\Bday\BDay1.wav]
;Birthday+2
IfCondition4=(MeasureTime > #StartTime# ) && (MeasureTime < #EndTime#) &&(MeasureDay = DateAfterBirthday2) && (00.00 <> #Birthday#)
IfTrueAction4=[PLAY \Special\Bday\BDay2.wav]

[MeasureImageSwap]
; Test for special day swap image and sound array as needed 
Measure=Calc 
;JokeDay
IfCondition= (MeasureDay = #JokeDay#)
IfTrueAction=[!SetOption ImageMeter ImageName #PicApril#][!SetOption TheAudioThatWillPlay String "#*SoundClickAprilFools*#"][!UpdateMeasureGroup "GroupRandom"]
;Anniversary
IfCondition2= (MeasureDay = #Anniversary#)
IfTrueAction2=[!SetOption ImageMeter ImageName #PicAnniv#][!SetOption TheAudioThatWillPlay String "#*SoundClickPartyHorn*#"][!UpdateMeasureGroup "GroupRandom"]
;Birthday
IfCondition3= (MeasureDay = #Birthday#)
IfTrueAction3=[!SetOption ImageMeter ImageName #PicBDay#][!SetOption TheAudioThatWillPlay String "#*SoundClickPartyHorn*#"][!UpdateMeasureGroup "GroupRandom"]
;Birthday+1
IfCondition4=(MeasureDay = DateAfterBirthday1)&& (00.00 <> #Birthday#)
IfTrueAction4=[!SetOption TheAudioThatWillPlay String "#*SoundClickPartyHorn*#"][!UpdateMeasureGroup "GroupRandom"]

[ImageMeter]
; Display code
Meter=Image
ImageName=Potato.png
H=200

LeftMouseUpAction=[PLAY [&TheAudioThatWillPlay]][!UpdateMeasureGroup "GroupRandom"]
