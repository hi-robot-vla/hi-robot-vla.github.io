
----

### Motivation

When was the last time you cooked a new dish? You look at the recipe, lay out the ingredients, and get to work. There is a little voice inside your head &mdash; "oh I forgot, I need to add the tomato." You think carefully through each step, periodically checking the recipe. Maybe your friend chimes in &mdash; "watch out, you'll burn it." Daniel Kahneman described two different ways that people solve problems, which he dubbed "System 1" and "System 2." System 1 feels instinctual and automatic; System 2 is deliberative and conscious. Cooking that new dish used System 2: that's the little voice you heard in your head. When you do something for the 100th time, that's System 1: it feels automatic and you hardly think about it.

Can we get our robots to "think" the same way, with a little "voice" that tells them what to do when they are presented with a complex task? We developed a system that we call the **H**ierarchical **I**nteractive **Robot** (**Hi Robot**) that allows us to incorporate vision-language-action (VLA) models, into a two-level inference process. In this system, a low-level VLA model serves as the instinctual, reactive "System 1" that can perform well-practiced tasks, and a high-level semantic vision-language model (VLM) plays the role of "System 2," reasoning through complex tasks and language interactions by "talking to itself." This System 2 high-level policy quite literally emulates that little voice, telling the robot how to break up complex tasks into intermediate steps, as shown in the example below.

<br>

<video width="100%" controls autoplay loop muted>
    <source src="./video/blog_video1_compressed.mp4" type="video/mp4">
</video>
<figcaption class="imgcaption" style="margin-top: 5px;">An example of our System 2 model breaking up a complex user prompt into simpler steps, which are then fed to a System 1 model to perform the task.</figcaption>

<br>

This high-level policy is itself a VLM, and it is trained to process complex prompts, observe the scene, and break up the task into bite-sized chunks, whispering individual steps ("pick up one piece of whole wheat bread") to the VLA model. This high-level policy can also incorporate real-time contextual feedback. If it is cleaning a table and the user says "that's not trash," the model understands what this means, grounding the object ("that") to the thing the robot is manipulating in the image, and correctly interpreting the implied instruction (i.e., "that" should not be placed into the trash, and therefore should be placed somewhere else) so that it can again "whisper" the correct intermediate step to the VLA model.

<br>

<video width="100%" controls autoplay loop muted>
    <source src="./video/blog_video2_compressed.mp4" type="video/mp4">
</video>
<figcaption class="imgcaption">Hi Robot responds to real-time language instructions, inferring the user's intent and situating it in the current scene.</figcaption>

<br>

----
### Why is this a good idea?

If both the high-level **Hi Robot** policy and the low-level VLA model are based on the same VLM, why is this hierarchical inference process actually advantageous? Just like language models have been shown to excel at complex problem-solving tasks if they are allowed to generate extra text to "think", **Hi Robot** can better process complex prompts and feedback if it is allowed to first break them up into simpler steps that the VLA model already understands how to do. There is also another more technical reason why this inference process is advantageous: the web-scale pre-training that we use to initialize VLMs trains the model to generate textual answers to prompts and questions that reference images and textual context. That means that, out of the box, such models are already quite proficient at responding to questions like, "in this picture, which object should the robot grasp next to clean the table?" **Hi Robot** is therefore able to better inherit the knowledge that the VLM builds up during web-scale pre-training. This is not so different from how you think: when you cooked that new recipe, you might have been thinking about things you learned from the recipe book, or from something your friend told you, or even something you saw in a cooking show &mdash; all knowledge that you gained from other sources besides your own personal embodied experience.

<br>

----
### Robots that talk to themselves

Inspecting the internal "thoughts" of **Hi Robot** when presented with a complex prompt, we can see how our system reasons through an intricate task defined by a user prompt.

<br>

<div style="display: flex; gap: 20px; margin-bottom: 20px;">
    <div style="flex: 1;">
        <figcaption class="imgcaption"></figcaption>
        <video width="100%" controls autoplay loop muted>
            <source src="./video/blog_post_video3_clean_everything_compressed.mp4" type="video/mp4">
        </video>
    </div>
    <div style="flex: 1;">
        <figcaption class="imgcaption"></figcaption>
        <video width="100%" controls autoplay loop muted>
            <source src="./video/blog_post_video3_only_trash_compressed.mp4" type="video/mp4">
        </video>
    </div>
</div>
<figcaption class="imgcaption">Hi Robot follows a complex prompt that requires modifying the behavior of the base model, which is trained to bus the table by putting all trash in the garbage and all dishes in the bin.</figcaption>

<br>


In this case, <P0 /> is trained to simply clean the table, putting all trash in the garbage and all dishes in the bin. Left to its own devices, <P0 /> would simply perform this task -- you've probably had the experience of doing something on "autopilot," where you find yourself performing a well-practiced task even as you forget what you were actually trying to do. But under the control of **Hi Robot**, <P0 /> can be adapted to follow this more intricate prompt, following the user's command as **Hi Robot** reasons through the modified commands that it should provide to <P0 />. Since these commands are produced in natural language, we can inspect them and see how the robot "talks to itself" to perform the task.

Interpreting a contextual comment from a user is a similar problem to interpreting a complex prompt, and in the same way that **Hi Robot** can parse intricate prompts, it can also incorporate real-time feedback even as it performs the task.

<br>

<video width="100%" controls autoplay loop muted>
    <source src="./video/blog_video4_compressed.mp4" type="video/mp4">
</video>
<figcaption class="imgcaption">Hi Robot incorporates corrections and feedback from the user.</figcaption>

<br>


----
### Where is this going?

Intelligent and flexible robotic systems need to not only perform dexterous tasks, but also understand their environment and reason through complex, multi-stage problems. While on the surface, **Hi Robot** is focused on interaction with users through prompts and feedback, the goal of this system is to ultimately endow robots with the same kind of "inner voice" that you hear when you are solving a difficult problem like cooking that new recipe. Interaction with people provides us with the most vivid illustration of the importance of this capability, but its implications go much deeper: robots that can think through complex problems and bring to bear knowledge they learned from web-scale pre-training will be much more flexible, will exhibit significantly better common sense reasoning, and in the long run provide us with much more natural assistance in open-world settings. They will be able to understand what it means when someone writes "do not erase" on the whiteboard, figure out that a person should not be disturbed if they are sleeping, and realize that fragile objects should be handled with care. These are all inferences we make every day based not only on our physical experience, but on things we learned from other people. LLMs and VLMs provide us with a tremendously powerful tool to learn this kind of knowledge from the Internet, but connecting this knowledge to physical systems such as robots in a seamless way presents major technical challenges. We hope that **Hi Robot** will serve as an important step in this direction.
