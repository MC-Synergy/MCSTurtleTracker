# MCS Turtle Tracker
![MCST Banner](https://github.com/Josian2004/s3-portfolio/blob/main/portfolio_images/MCSTbanner.png)

## What does it do?
MCST gives MCS players the ability to track every move of the turtles on the server. You can see the real-time location, status and fuel level of the turtles so that when something goes wrong with a turtle you can easily see where it is.

Turtles also have the ability to send messages to the application, it can send, for example, a message when it needs fuel or when it starts a farming run. When a turtle crashes it will also send an error message and the status will change to "Error". This way you can see in one quick look if a turtle has crashed and needs player assistance.

## Features
- See live location (inc. dimension).
- See current status.
- See sent messages.
- See if a turtle has crashed or needs some other form of assistance.
- See the fuel level.

### Coming Soon!
- See inventory.
- Mobile app with push notifications.
- Train Tracker, the ability to track all of the minecarts currently active on the metro system.
- Remotely enable/disable turtles and complete systems.
- Send commands/perform basic actions to turtles.

If you have an awesome idea, feel free to add it in an issue!

## How can I use it?
The app is open for the public so even non MCS players can see it. As a visitor, directly head to the [MCS Turtle Tracker](https://turtletracker.mcsynergy.nl).

Some features however are only available for MCS players, please head to the [MCS Portal](https://portal.mcsynergy.nl/) and login with your account.

## Why did I make it?
I ([Josian van Efferen](https://www.linkedin.com/in/josianvanefferen/)) created it for a school project, during semester 3 of HBO ICT & Software Engineering I had to build a full stack web-application so I made this. A few other important goals of this semester where CI/CD, quality assurance and research, you can find more information about my school project in my [portfolio](https://github.com/Josian2004/s3-portfolio/blob/main/Individual/README.md#mcsturtletracker).

## How can I implement this?
Every MCS player can ofcourse add their own systems and turtles to this service, so you will never lose your turtle again. It's pretty easy to begin with this system but there are some littles pieces of code you need to add to your script to take full advantage of the service.

### Step 0 - Modem
The most important requirement for the service to work is that your turtle or computer ***needs*** an ***ender*** modem! Without the ender modem the service will be completely useless. If your turtle has no more free peripheral slots you will need to make some changes in your system so that every turtle has an ender modem.

### Step 1 - Library
You will need the turtletrackerlib.lua for this to work. There are two different libraries, one for turtles and one for (pocket) computers.

Add this to the top of your ***Turtle*** script:
```lua
require("lua scripts.Josians Shit.MCSTurtleTracker.turtletrackerlib")
```

Add this to the top of your ***Computer*** script:
```lua
require("lua scripts.Josians Shit.MCSTurtleTracker.turtletrackerlib_computer")
```

### Step 2 - Initialize
You need to make sure that the whole flow of the scripts is in a main function, so the main script loop should also be in the main function.

***Incorrect:***
```lua
local function main()
  -- main script stuff
end

while true do
  main()
end
```

***Correct:***
```lua
local function main()
  while true do
    -- main script stuff
  end
end

main()
```

When this is correct, you can start to initialize MCST by replacing the call to your main function with the Init method like seen in the next example. The second parameter is the ID of the system that the turtle is part of, you can see the list of systems on our [MCS Systems API Front-end](https://portal.naamdorpboot.xyz/). The third parameter is if you want to allow MCST to print information to the turtle, this information can be that MCST has started and that it has succesfully established a connection. The fourth parameter is for if you want that every message and status update sent to MCST is printed in the turtle.

Change
```lua
main()
```
to
```lua
MCST_Init(main, 0, true, true) -- id = 0 means that the turtle has no system.
```
Congratulations! Your turtle should now be visible in the app.
However the turtle doesn't send messages or update their status, to implement this, please follow the next steps.

### Step 3 - Messages
Turtles can send messages to the app, there are four types of messages: Info, Warning, Error and Junk. You can send messages at certain places in your script by adding these methods.
```lua
MCST_SendInfo("Info Message!")
MCST_SendWarning("Warning Message!")
MCST_SendError("Error Message!")
MCST_SendJunk("Junk Message!")
```
Junk messages are purely used for debug, for example you can send a junk message every time a turtle starts a farm run. These junk messages are automatically hidden in the app and need to be manually enabled to be seen.

### Step 4 - Status
The turtle also sends their current status to the app. There are a few pre-ditermined status but you can also set your own custom status. These kinda work the same as messages but the selected status remains until you select another status so make sure that the status is always kept up-to-date in the flow of your script.

```lua
MCST_SetDoneStatus()
MCST_SetFarmingStatus()
MCST_SetWaitingStatus()
MCST_SetErrorStatus() -- this will show as red in the app and will be automatically set when the turtle crashes
MCST_SetRefuelingStatus()
MCST_SetNeedPlayerStatus() -- this will show as orange in the app
MCST_SetManuallyTerminatedStatus() -- this will show as orange in the app and 
                              -- will be automatically set when the turtle is manually terminated
MCST_SetReturningStatus()
MCST_SetEmptyingStatus()
MCST_SetCustomStatus("Custom Status")
```

### Step 5 - Done
The service should now be completely operational for your turtle, head over to [MCST](https://mcst.josian.nl) to see it in action. If it doesn't work or you don't understand how to implement it, please take a look at the [SuperFurnace/LavaCollector](https://github.com/MC-Synergy/MCS-Scripts/blob/master/lua%20scripts/SuperFurnace/LavaCollector.lua) for an example or send me a message on Discord.

The service is still in development so if you might encounter a bug, please open a ticket and I will try to fix it as soon as possible (prob eta 2024).
