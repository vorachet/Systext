# Systext (Temporary repository)

Systext is a textual domain-specific language for systems modeling and tradeoff analysis developed as a research project from [School of Information Technology, King Mongkut's University of Technology Thonburi](https://www.sit.kmutt.ac.th/en/)

The main design goal of Systext is to provide lightweight systems modeling that allows users to perform the following system modeling tasks

Definition Models

* structure and behavior
* finite state machine
* constraints and constraint blocks

Task Models

* create structural view
* create event view
* create constraint view
* create finite state machine
* tradeoff analysis

# Examples

## Structure and Evnet

### Output

### Input

[structureAndEventExamples_ByModeler.syt]

```
@Model PillcamModel

PillcamApplication {
   detectEsophagealDiseases() {}
   detectGastrointestinalReflexDiseases() {}
   detectBarreffEsophagus() {}
   detectCrohnDisease() {}
   detectSmallBowel() {}
     detectTumors() {}
     detectSmallBowelInjury() {}
     detectCeliacDisease() {}
     detectUlcerative() {}
     detectColitis() {}
     uses solution:DisposablePillCam [1..-1]
}
DisposablePillCam {
  String developer "http://pillcamcolon.com/"
  captureAndSendImages() {}
  has users:Doctor [1..-1]
  contains opticalDome:OpticalDome [1..1]
  contains lensHolder:LensHolder [1..1]
  contains lens:Lens [1..1]
  contains led:LEDLight [4..4]
  contains imgSensor:CMOSImageSensor [1..1]
  contains batteries:Battery [2..2]
  contains transmitter:ASICTransmitter [1..1]
  contains antennae:Antennae [1..1]
}

OpticalDome {

}
LensHolder {

}
Lens {

}
LEDLight {

}
CMOSImageSensor {

}
Battery {

}
ASICTransmitter {}
Antennae {

}
Doctor {
  examColon() {}
  getImages() {}
  uses technology:PillcamApplication [1..1]
  uses pillCAMs:DisposablePillCam [1..-1]
  uses recordingDevice:WirelessImgRecorder [1..1]
  has patients:Patient [1..-1]
}
ImageOverRFCommunication {}
WirelessImgRecorder {
  receiveImages() {}
    showImages() {}
}
Patient {
  swallow() {}
}
Image{}
SignalEvent Doctor.examColon()
 > DisposablePillCam > Patient.swallow()
SignalEvent DisposablePillCam.captureAndSendImages()
 > ImageOverRFCommunication > WirelessImgRecorder.receiveImages()
CallEvent Doctor.getImages()
 > Image > WirelessImgRecorder.showImages()

WholeSystemView = createStructureView(
  PillcamApplication
  DisposablePillCam
  Patient
  Doctor
  WirelessImgRecorder
  ImageOverRFCommunication
  Image
  OpticalDome
  LensHolder
  Lens
  LEDLight
  CMOSImageSensor
  Battery
  ASICTransmitter Antennae
)

TechnologyApplicationContext = createStructureView(
  DisposablePillCam
  Patient
  Doctor
  WirelessImgRecorder
  ImageOverRFCommunication
  Image
)
DoctorEvents = createEventView(Doctor)
PatientEvents = createEventView(Patient)
DisposablePillCamEvents = createEventView(DisposablePillCam)
WirelessImgRecorderEvents = createEventView(WirelessImgRecorder)
```

[structureAndEventExamples_ByUser.syt]
```
@Model PillcamByModelUser

StructureAndEventDemoDoc = createDocument(
  title: "Structure and Event Model Examples - Pillcams"
  author: "Vorachet Jaroensawas"
).content(

  {acronym: ASIC "Application-Specific Integrated Circuit"}

  == "Structure and Event Model Examples" ==

  -- "WholeSystemView" --
  {structureView: PillcamModel.WholeSystemView}

  -- "TechnologyApplicationContext" --
  {structureView: PillcamModel.TechnologyApplicationContext}

  -- "DoctorEvents" --
  {eventView: PillcamModel.DoctorEvents}

  -- "PatientEvents" --
  {eventView: PillcamModel.PatientEvents}

  -- "DisposablePillCamEvents" --
  {eventView: PillcamModel.DisposablePillCamEvents}

  -- "WirelessImgRecorderEvents" --
  {eventView: PillcamModel.WirelessImgRecorderEvents}

)

```

## Finite State Machine

### Output

https://cdn.rawgit.com/vorachet/Systext/9ddf28b1/Examples/FSMSimulationDemoDoc.html

### Input

[fsmExamples_ByModeler.syt](https://github.com/vorachet/Systext/blob/master/Examples/fsmExamples_ByModeler.syt)

```
@Model FSMExamplesByModeler
Elevator {
    up() {}        down() {}
    redOn() {}     redOff() {}
    greenOff() {}  greenOn() {}
    Ground InitialState [
      up() > First
      entry( redOn greenOff )
    ]
    First [
      down() > Ground
      entry( redOff greenOn )
    ]
}
StopWatch {
  turnOn() {}        turnOff() {}
  resetDisplay() {}  ensureTimer() {}
  syncDisplay() {}   start() {}
  stop() {}          stopTimer() {}
  reset() {}         split() {}
  unsplit() {}       freezeDisplay() {}
  OFF InitialState [ turnOn() > READY ]
  READY [
    start() > RUNNING
    turnOff() > OFF
    entry( resetDisplay )
  ]
  RUNNING [
    stop() > STOPPED
    split() > PAUSED
    turnOff() > OFF
    entry(ensureTimer syncDisplay)
  ]
  STOPPED [
    reset() > READY
    turnOff() > OFF
    entry(stopTimer)
  ]
  PAUSED [
    stop() > STOPPED
    unsplit() > RUNNING
    turnOff() > OFF
    entry(freezeDisplay)
  ]
}
StopWatchFSMView = createFSMView(StopWatch)
ElevatorFSMView = createFSMView(Elevator)

```

[fsmExamples_ByUser.syt](https://github.com/vorachet/Systext/blob/master/Examples/fsmExamples_ByUser.syt)

```
@Model FSMExamplesByUser

FSMSimulationDemoDoc = createDocument(
  title: "FSM Simulation Examples"
  author: "Vorachet Jaroensawas"
).content(

  {acronym: FSM "Finite State Machine"}

  == "FSM Simulation Examples" ==

  -- "ElevatorFSM" --

  {fsmView: FSMExamplesByModeler.ElevatorFSMView}

  -- "StopWatchFSM" --

  {fsmView: FSMExamplesByModeler.StopWatchFSMView}

)

```

# License

License and supporting tool will be provided soon

Copyright (c) 2018 School of Information Technology, King Mongkut's University of Technology Thonburi