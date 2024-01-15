---
title: "An empty day"
date: 2023-10-15T18:15:41Z
draft: false
---

###### I messed up the date on yesterday’s post, I have fixed it now.

This is going to be a short update since today was a very empty day. I spend most of today playing Cuphead. In the spare time, I started solving AOC 2022 problems. I’ve decided that I’ll try to solve these problems in both Rust as well as C. A medium-sized goal, but I’m excited to explore both of the languages properly. The challenge is also to do this while cutting off internet, but I ran into some hiccups therefore I’ll stop using internet maybe 2-3 days into the challenge? Anyways, here is the C implementation for the first half of the first problem:

```c
#include <stdio.h>

int total_calorie_max = 0;
FILE *elf_data;

int main() {
    
    const int line_len = 100;
    char line[line_len];
    int calorie_val = 0;
    int elf_calorie_total = 0;
    elf_data = fopen("elf_data_final.txt","r");
    char next_char;
    while(fgets(line, sizeof(line), elf_data) != NULL){
        if(line[0] == '\n' || line[0] == '\0'){
            if (elf_calorie_total > total_calorie_max) {
                total_calorie_max = elf_calorie_total;
            }
            elf_calorie_total = 0;
        }
        else {
            sscanf(line, "%d", &calorie_val);
            elf_calorie_total += calorie_val;
        }
    }
    printf("Max Calorie value held by an elf is %d \n", total_calorie_max);
    fclose(elf_data);
}
```

Here is the same implementation in rust. I must admit I took a lot of reference from online, something that I should not do.

```rust
use std::fs::File;
use std::io::{self, BufRead};
use std::error::Error;

fn main() -> Result<(), Box<dyn Error>> {
    let file = File::open("elf_data.txt")?;
    let reader = io::BufReader::new(file);
    let mut total_calorie_max = 0;
    let mut elf_total_calorie = 0;

    for line in reader.lines() {
        let line = line?;
        if line.trim().is_empty() {
            if elf_total_calorie > total_calorie_max {
                total_calorie_max = elf_total_calorie;
            }
            elf_total_calorie = 0;
        } else {
            let item_calorie = line.trim().parse::<i32>()?;
            elf_total_calorie += item_calorie;
        }
    }

    println!("Total calorie max: {}", total_calorie_max);
    Ok(())
}
```

I might’ve gotten into a lot of trouble trying to discern if a line was empty or not. Finally, I simply extract the line and  check for a `\n` character. I have forgotten all my C…..

I also tried a new game today, called Haven Park. The game feels like a sister to the game “A short hike” but with total focus on exploration and very little story. I might write a review for it in the coming time.

Rust seems very confusing at the start, the concept of ownership feels very alien. Nevertheless, I am excited to learn more about it.

Today felt really slow and lonely. I was also angry at my friends because yesterday, they went out without me. I plan to ignore them tomorrow in college, I don’t want anything to do with them.

Tomorrow, I plan to attend a club meeting, and get back home in time to study / work on my side projects. Until then, toodles :3
