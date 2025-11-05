let inBed = false
let alarmDone = true;
let alarmTime = 0;
const maxAlarmTime = 9;
let bendSensor = 0
// turns alarm on with custom timer and turns off the alarm value off when done (jayden en bastian)
function turnAlarmOn(time: number) {
    alarmDone = false;
    for (let i = 0; i <= time; i++) {
        pause(1000)
        light.setPixelColor(i, Colors.Blue);
        if (i === time && inBed) {
            music.baDing.loop()
        }
        console.log(i)
    }
    alarmDone = true;
}

// Checks if someone is in bed and resets the alarm if you lie down again within a certain time. (Alex and Jayden)
function layInBed() {
    inBed = true;
    if (inBed && alarmDone) {
        turnAlarmOn(1)
    }
    console.log(inBed);
}
// checkt of iemand in bed ligt en zet het alarm uit zo niet en reset het alarm (alex)
function notInBed() {
    inBed = false
    music.stopAllSounds()
    console.log(inBed)

    if (alarmDone) {
        for (let i = 0; i <= alarmTime; i++) {
            light.setPixelColor(i, Colors.Green);
        }
    }
}
// Makes the flex sensor work and calls functions. (alex)
forever(function () {
    bendSensor = pins.A3.analogRead()
    if (bendSensor > 850) {
        layInBed();
    } else {
        notInBed();
    }
})


input.buttonA.onEvent(ButtonEvent.Click, function () {
    if (alarmTime > 0) {
        turnAlarmOn(alarmTime);
    }
})

input.buttonB.onEvent(ButtonEvent.Click, function () {
    if (alarmDone) {
        alarmTime++
        if (alarmTime > maxAlarmTime) {
            alarmTime = 0;
            light.clear();
        }
    }
})



