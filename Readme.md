# Systext (Temporary repository)

Systext is a textual domain-specific language for systems modeling and tradeoff analysis developed as a research project from [School of Information Technology, King Mongkut's University of Technology Thonburi](https://www.sit.kmutt.ac.th/en/)

The main design goal of Systext is to provide lightweight systems modeling that allows users to perform the following system modeling tasks

Definition Models

* structure and behavior
* finite state machine
* constraints and constraint blocks (the composition of constraints) with mathematical expressions, multiple output and shared inputs support

Task Models

* create structural view
* create event view
* create constraint view
* create finite state machine
* tradeoff analysis

# Examples

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