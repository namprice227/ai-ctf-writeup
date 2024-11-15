# Fifty Cats CTF Write-Up

## Challenge Name: Fifty Cats  
**Category:** Easy  
**Objective:** Generate an image with exactly 50 cats using the AI image generator.

---

## Challenge Description

The goal of this challenge is to generate an image containing exactly **50 cats**. At first glance, this seems straightforward, but I quickly discovered a range of quirks and technical nuances in the AI image generation system.

---

## Observations

### Initial Results:
1. **Consistent Results Around 9 Cats**:  
   Initially, the AI generator consistently produced images with 9 cats, even when tweaking the prompt. This was perplexing because the system seemed unable to generate the desired number.

2. **Occasional Glitch**:  
   Occasionally, the AI would count a much higher number of cats (e.g., 70 cats), even when only 9-12 cats were visibly present. I believe this happened due to overlapping cats being mistakenly identified as multiple entities.

---

## Key Metrics: `conf_threshold` and `iou_threshold`

The program provided two key parameters for tweaking the AI's cat recognition:

1. **`conf_threshold` (Confidence Threshold)**:  
   This controls the minimum confidence level for the AI to recognize an object as a "cat." Lowering this threshold makes the system less strict, causing it to recognize non-cat features as cats.

2. **`iou_threshold` (Intersection Over Union Threshold)**:  
   This determines how much overlap between detected objects is acceptable before they are considered the same entity. Lowering this threshold allows more overlapping objects to be recognized as separate cats, even if they're not.

By decreasing both thresholds, I found that the image generator would overestimate the number of cats. This behavior became crucial in achieving 50 "recognized" cats.

---

## Strategy

### 1. Automating Queries
Manually generating images was inefficient due to API errors such as:
- Timeouts
- Server busy responses
- No connection

To overcome these challenges, I developed a Python script that:
- Automatically loops through queries.
- Handles errors gracefully by retrying when timeouts or connection issues occur.

### 2. Optimizing Parameters
By tweaking the `conf_threshold` and `iou_threshold` to lower values, the AI became more lenient in recognizing cats. This adjustment often resulted in inflated counts, sometimes exceeding 50 cats. The metric I find turns out to be (0.1, 0.1)

### 3. Prompt engineering
The prompt needs to have words like "clear, defined, hyper-realism, high resolution, upscaling" to make the AI generate the cat images more clear. 

My prompt used is: Generate a grid of many white cats faces, smooth face, symmetrical face, **clear face**, **defined face**, *hyper-realism*, **high resolution**, *upscaling*.

