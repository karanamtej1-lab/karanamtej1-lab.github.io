---
title: "nanoGPT from Scratch"
date: 2026-07-11
summary: "A character-level GPT built from the ground up in PyTorch. A bigram baseline, then the full Transformer, trained to write fake Shakespeare."
tags: ["python", "pytorch", "ai"]
stack: ["Python", "PyTorch", "Transformers", "Apple MPS"]
links:
  - name: "Source on GitHub"
    url: "https://github.com/karanamtej1-lab/nanogpt"
---

## Overview

A GPT built from nothing but tensors. It reads all of Shakespeare one character
at a time, learns the patterns, and writes new lines that look like the real
thing. No libraries doing the hard part. Every piece of the Transformer is
written out by hand.

## How it works

- A bigram model comes first. Each character predicts the next from a lookup
  table. It sets up the training loop and the loss, and it writes gibberish.
  That gibberish is the baseline everything else has to beat.
- The full model is a decoder-only Transformer. Masked self-attention lets each
  character look back at the ones before it. Multiple attention heads run in
  parallel, then a feed-forward network does the per-token thinking.
- Blocks stack six deep. Residual connections and layer normalization keep the
  gradients healthy on the way down. Positional embeddings tell the model where
  each character sits, since attention alone has no sense of order.
- Training runs on Apple MPS. Validation loss falls from 2.5 at the bigram
  baseline to roughly 1.48, and the samples turn into readable fake Shakespeare.

## What I learned

Attention is a weighted average, and the weights are learned. That one idea
carries the whole architecture. The scary math is a matrix multiply, a mask so
the model cannot read the future, and a softmax. Writing it by hand instead of
importing it is the difference between using a Transformer and understanding
one.

## Status

Built and trained. A study project I plan to extend to word-level tokens.
