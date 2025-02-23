---
layout: post
title: AI Engineering - A short (stupid) guide for a Software Engineer
categories: blog
tags: AI LLM llama
---

When I wrote the [blog post](/blog/2023/12/29/software-engineer-20-year-journey.html) of my nearly two decades of experience working in the software industry in Dec 2023 [0] , I predicted that Artificial Intelligence (AI) and Large Language Models (LLMs) are going to have a huge impact in software development but did not anticipate it to be this revolutionary. 

AI and LLMs are not just the tools or co-pilot anymore but they are becoming the essential part of software delivery. In 2018 when LLMs like chatGPT were first introduced, for average Engineers like myself, it looked like magic but as I read and learned each step in the engineering process it started to make sense. This new engineering is now starting to emerge as Artificial Intelligence (AI) Engineering. 

So this is my attempt to translate software engineering flow to Artificial Intelligence (AI) Engineering. Since this is my original idea it could be a stupid way of looking into this. 

As we all know the simplistic view of current software development is a team of software engineers write code based on business requirements and success metrics, pass it through a compiler that generates the output, expected behavior is tested, packaged it in a container and deployed to the cloud or data centres for end customers to use it. 

![Image showing simplifiied view of software engineering](/assets/images/blog-software-flow.png "Image showing simplifiied view of software engineering")

*Figure 1: Simplified view of software engineering*

In my view, the equivalent of this flow for AI Engineering is below:

![Image showing simplifiied view of software engineering](/assets/images/blog-AIEngineering.png "Image showing simplifiied view of software engineering")

*Figure 2: Simplified view of AI engineering*

I will now briefly discuss each step. 

# Data

Data is the equivalent of codes in AI Engineering. Instead of software engineers writing code, data engineers will prepare data in the form of Pre-training by going through various stages of cleaning data as we know the quality of the data will determine how good the response is for LLMs. 

Here is the high level flow of this pre-training data [1]


![Image showing pre-training data flow](/assets/images/blog-PreTraining.jpg "Image showing pre-training data flow]")


I think the paradigm shift in AI Engineering from software engineering is that data already contains the subset of code. So the need of Software Engineers to contribute to the general codebase is reducing.

Creating quality data through crawling or synthetically is the most important part of AI Engineering and it is not a small task with many challenges. 


## Distillation

This is another clever and shortcut way to train models. 

Distillation is the process of training smaller models such as Deepseek R1 Distilled models initialized from large models like llama and distilled from data synthesized by R1 and R1-Zero. Although the original idea of AI Distillation comes from a paper written by the father of AI, Geofrrey Hinton [2], Deepseek fully leveraged this technique to make a breakthrough in early 2025. 


# Foundational Models

Foundational Models like LLMs are an equivalent of compilers in AI Engineering. 

The high level architecture of Transformer based LLMs as drawn by Anthropic Claude 3.5 Sonnet is as below : 

![Image showing transformer based LLM architecture](/assets/images/blog-transformer-arch.png "Image showing transformer based LLM architecture]")

We will not go into details of Transformer based LLM architecture that are most used in today’s popular foundation models but if you want to deep dive you can read the seminal paper published by google *Attention is all you need* [3]

When running a llama model locally and debugging, gives you it’s standard protocol as below: 

```
llm_load_print_meta: format       	= GGUF V3 (latest)
llm_load_print_meta: arch         	= llama
llm_load_print_meta: vocab type   	= BPE
llm_load_print_meta: n_vocab      	= 128256
llm_load_print_meta: n_merges     	= 280147
llm_load_print_meta: vocab_only   	= 1
llm_load_print_meta: model type   	= ?B
llm_load_print_meta: model ftype  	= all F32
llm_load_print_meta: model params 	= 3.21 B
llm_load_print_meta: model size   	= 1.87 GiB (5.01 BPW)
llm_load_print_meta: general.name 	= Llama 3.2 3B Instruct
llm_load_print_meta: BOS token    	= 128000 '<|begin_of_text|>'
llm_load_print_meta: EOS token    	= 128009 '<|eot_id|>'
llm_load_print_meta: EOT token    	= 128009 '<|eot_id|>'
llm_load_print_meta: EOM token    	= 128008 '<|eom_id|>'
llm_load_print_meta: LF token     	= 128 'Ä'
llm_load_print_meta: EOG token    	= 128008 '<|eom_id|>'
llm_load_print_meta: EOG token    	= 128009 '<|eot_id|>'
llm_load_print_meta: max token length = 256
```

Since I am comparing LLM with a compiler, let’s look at how a Java compiler works. Since most compiler are not able to draw its own architecture, I again asked Anthropic Claude 3.5 Sonnet to give me the high level architecture :

![Image showing Java compiler architecture](/assets/images/blog-java-compiler-arch.png "Image showing Java compiler architecture]")

At least to me they look very similar. At the high level a compiler is converting code into bytecode while LLM is converting data into token. 

Here is an equivalent of a bytecode generated in java compiler:

```
Compiled from "HelloWorld.java"
class HelloWorld {
  HelloWorld();
	Code:
   	0: aload_0
   	1: invokespecial #1              	// Method java/lang/Object."<init>":()V
   	4: return

  public static void main(java.lang.String[]);
	Code:
   	0: getstatic 	#2              	// Field java/lang/System.out:Ljava/io/PrintStream;
   	3: ldc       	#3              	// String Hello, World!
   	5: invokevirtual #4              	// Method java/io/PrintStream.println:(Ljava/lang/String;)V
   	8: return
}
```

# AI Evaluation


In Software Engineering, test cases are finite so they can be largely covered through semantic and lexical tests in the form of regressions, integrations and acceptance with some input from humans. 


In AI Engineering, it is equivalent to having an AI Evaluation system based on the business and non functional requirements. However, AI evaluation is more challenging due to non-deterministic nature for which there will be different output for the same input so multiple evaluation techniques often with human judgement is important esp in domains where correctness is more crucial. 


# Customization

Packaging is when you put the bytecode in the container like docker or kubernetes to make it ready for deployment based on the needs of the customer. 

In AI Engineering, customization is the process of adapting or modifying a pre-trained language model to better serve specific use cases, domains, or requirements. Fine tuning, prompt engineering, Retrieval Augment Generation (RAG), context window optimization etc. are different approaches for customization. 


# Inference

Deployment is the final step in software engineering when users start using the service. 

Similarly, inference is the final step when LLM attempts to give the statistically best response or predictions based on input provided. At this step, focus is on operational as well as engineering optimizations to provide the best user experience at affordable cost. 

For e.g. inference Llama 3 405B model optimizes inference with efficient pipeline parallelism and FP8 quantization [4].

FLOPS (Floating Point Operations Per Second) has emerged as a critical metric for quantifying the computational demands and capabilities of LLMs. It serves as a currency for LLM.


# Conclusion

In my opinion, AI engineering will not replace compilers and software development. While traditional systems are good at deterministic tasks with clear rules, AI handles ambiguous and non-deterministic data-rich problems. 

As systems grow in complexity, the most robust solutions will strategically combine both approaches—using traditional engineering for infrastructure and AI for adaptive intelligence. This synergy defines the next era of computational problem-solving.


# References

[0] [https://royalghost.github.io/blog/2023/12/29/software-engineer-20-year-journey.html](https://royalghost.github.io/blog/2023/12/29/software-engineer-20-year-journey.html) 

[1] [https://huggingface.co/spaces/HuggingFaceFW/blogpost-fineweb-v1](https://huggingface.co/spaces/HuggingFaceFW/blogpost-fineweb-v1) 

[2] [https://arxiv.org/abs/1503.02531](https://arxiv.org/abs/1503.02531) 

[3] [https://arxiv.org/pdf/1706.03762](https://arxiv.org/pdf/1706.03762) 

[4] [https://ai.meta.com/research/publications/the-llama-3-herd-of-models/](https://ai.meta.com/research/publications/the-llama-3-herd-of-models/)