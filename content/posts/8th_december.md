---
title: "8th december"
date: 2022-12-08T01:49:41+05:30
draft: false
---

It’s a bit weird how people can take others for granted. The friend slowly goes unchecked, and one day they simply disappear. I hope I am that friend.

Don’t get me wrong, I do enjoy human company, but my current company is bad. I care too much about them, and the feeling isn’t returned. That’s why I have decided to slowly phase them out of my life.

That’s it for the day, nothing much happened apart from that. Now comes the important part:

## Tech Exploration!

These sections of my blog will be the most worth reading! I promise to deliver some good stuff here since I love technology!

Since this is the first Tech Exploration section, I’ll talk a bit about what I’m interested in. I am currently working on RTL design on FPGAs, and embedded circuits on the side. Aside from that, my current undergraduate major is Electrical Engineering. No, it’s not the fun one with the ICs and the milli-Ampere currents. It’s about the overhead electrical grid lines and the transformers and the DC Machines. Not fun at all.

So, since today is all about new beginnings, I have started with my daily HDL Bits problem solving, which I have been meaning to start doing for a long time! Today I solved about nine problems on there and have nicely covered the topics of D Flip Flops. I wonder what more complicated circuits can be made using them.

Keeping the spirit of new beginnings alive, I also revisited my RISC-V 32-bit CPU and am trying to produce a way to pipeline it, so that an instruction is executed every cycle, rather than four of them. I have noticed that I have almost forgotten everything about the project, which would not have happened if I had a blog back then!
On the subject of pipelining, CPU and understanding it all

My CPU currently has four stages:

```verilog
case(state)
    2'b00, 2'b01:
        //Enable the ALU on the basis of instruction
    2'b10: begin
        //disable the ALU, and write to the memory if instruction dictates so. Also pass the next PC value if we are to jump to an instruction
    2'b11: begin
        //get ready for the next cycle.
    end
endcase
```

As you can see, the 2’b01 is going to waste since nothing new is happening in this cycle. I can totally work in three cycles, but then I cannot take advantage of the bit overflow that resets the state back to zero.

So, for a four staged CPU, an efficient approach would be:

    Enable ALU and perform instruction.

    Disable ALU, write to memory.

    Send jump values.

    Reset jump values.

After that, I shall begin with the pipelining.

Pipelining works in the way that each stage keeps on working and no stage is idle, so I shall have to feed data into the stages (and the part they are working with) constantly. My poor stage 4 is going to break. Maybe I can turn it into a 2 stage CPU, if I change the way jumps are handled?

So that totally depends on my Program Counter. I cannot make any changes since I do not have electricity at the moment. My GPU sucks big time, and everything is completely lagging.

## What do I plan to do for the rest of the day and tomorrow?
For the rest of the day,

I plan to get some studies done. I haven’t touched a book in a while. Or else, I’ll finish writing mails for foreign Research Internships!
For tomorrow,

I plan to get my robotics kit! My team has qualified for stage two of a national level robotics competition! I did a major part and was the team leader! I will get a DE0 Nano with a Cyclone IV FPGA, let’s go!

Apart from that, I aim to finalize the details of an exhibition our robotics club will be keeping. Hopefully that event is executed nicely!

I have mosquitoes biting me and I am going overtime ( I am only allowed to write for 45 minutes every day) See you tomorrow!