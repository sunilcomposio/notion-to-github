Everyone’s talking about Ministral 3 3B, so I wanted to see what the hype is about. 🤨

Let's test it properly. We’ll start with the fun part and run it directly in the browser using WebGPU, fully local.

Then we’ll switch to the practical setup and run a quantized version with Ollama, plug it into Open WebUI, and test real tool calling. First with small local Python tools, then with remote MCP tools via Composio.

We will cover a few specs and then move on to practical tests, so let's jump in.

---

## What’s Covered?

In this hands-on guide, you’ll learn about the Ministral 3 3B model, how to run it locally, and how to get it to perform **real tool calls** using Open WebUI, first with local tools and then with **remote MCP tools via Composio**.

**What you will learn: ✨**

- What makes Ministral 3 3B special

- How to run the model locally using Ollama (including pulling a quantized variant)

- How to launch Open WebUI using Docker and connect it to Ollama

- How to add and test local Python tools inside Open WebUI

- How to work with remotely hosted MCP tools in Open WebUI

> ⚠️ **NOTE:** This isn’t a benchmark post. The idea is to show a practical setup for running a small local model with real tools, then extending it with remote MCP servers.

## What's so Special?

Ministral 3 3B is the smallest and most efficient model in the Ministral 3 family. Mistral 3 includes three state-of-the-art small dense models: 14B, 8B, and 3B, along with Mistral Large 3, which is the most capable model to date from Mistral. All models in this family are open source under the Apache 2.0 license, which means you can fine-tune and use them commercially for free.

But the topic of our talk is the **Ministral 3 3B model**. At such a small size, it comes with function calling, structured output, vision capabilities, and most importantly, it is one of the first multimodal models capable of running **completely locally** in the browser with WebGPU support.

As Mistral puts it, this model is both compact and powerful. It is specially designed for edge deployment, offering insanely high speed and the ability to run completely locally even on fairly old or low-end hardware.

Here is the model’s token context window and pricing.

- **Token Context Window:** It comes with a 256K token context window, which is impressive for a model of this size. For reference, the recent Claude Opus 4.5 model, which is built specifically for agentic coding, comes with a 200K token context window.

- **Pricing:** Because it is open source, you can access it for free by running it locally. If you use it through the Mistral playground, pricing starts at $0.1 per million input tokens and $0.1 per million output tokens, which is almost negligible. It honestly feels like the pricing is there just for formality.

Besides its decent context window and fully open-source nature, these are the major features of Ministral 3 3B.

- **Vision:** Enables the model to analyze images and provide insights based on visual content, in addition to text.

- **Multilingual:** Supports dozens of languages, including English, French, Spanish, German, and more.

- **Agentic:** Offers strong agentic capabilities with native function calling and JSON output, which we will cover shortly.

- **Local:** Runs completely locally in your browser with WebGPU support.

Here is a small demo of the model running directly in the browser:

To actually get a feel for running a model locally in the browser, head over to this Hugging Face Space: [mistralai/Ministral_3B_WebGPU](https://huggingface.co/spaces/mistralai/Ministral_3B_WebGPU)

> 💡 **NOTE:** For most users, this will work out of the box, but some may encounter an error if WebGPU is not enabled or supported in their browser. Make sure WebGPU is enabled based on the browser you are using.

When you load it, the model files, roughly 3GB, are downloaded into your browser cache, and the model runs 100 percent locally with WebGPU acceleration. It is powered by [Transformers.js](https://huggingface.co/docs/transformers.js/en/index), and all prompts are handled directly in the browser. No remote requests are made. Everything happens locally.

How cool is that? You can run a capable multimodal model entirely inside your browser, with no server involved.

---

## Running Ministral 3 3B Locally

In the above example, you see how the model does such an amazing job with vision capabilities (live video classification). Now let's see how good this model is at making tool calls. We will test it by running the model locally on our system.

For this, there are generally two recommended approaches:

- **vLLM**: Easy, fast, and cheap LLM serving for everyone.

- **Good old Ollama**: Chat & build with open models.

You can go with either option, and generally speaking, the vLLM approach is a lot easier to get started with, and that's what I'd suggest, but...

I kept hitting a CUDA out-of-memory error, so I went with Ollama with the quantized model. I have had a great experience with Ollama so far. The model will be good enough for our use case demo.

### Step 1: Install Ollama and Docker

If you don't have Ollama installed already, install it on your system by following the documentation here: [Ollama Installation Guide](https://ollama.com/download).

It's not compulsory, but we will use the OpenWebUI through the Docker container, so if you plan to follow along, make sure you have Docker installed.

You can find the Docker installation guide here: [Docker Installation](https://docs.docker.com/engine/install/).

### Step 2: Download Ministral 3 3B Model and Start Ollama

Now that you have Ollama installed and Docker running, let's download the [Ministral 3 3B model](https://ollama.com/library/ministral-3:3b) and start Ollama.

```bash
ollama pull ministral-3:3b
```

> ⚠️ **CAUTION**: If you don't have sufficient VRAM (Virtual RAM) and decent specs on your system, your system might catch fire when running the model. 🫠

If so, go with the quantized model instead as I did.

```bash
ollama pull ministral-3:3b-instruct-2512-q4_K_M
```

Now, start the Ollama server with the following command:

```bash
ollama serve
```

Once the model is downloaded and the server is running, you can quickly test it in the terminal itself.

```bash
ollama run ministral-3:3b-instruct-2512-q4_K_M "Which came first, the chicken or the egg?"
```

If you get a response, you are good to go.

### Step 3: Run Ollama WebUI

To just talk with the model, the CLI chat with `ollama run` works perfectly, but we need to add some custom tools to our model.

For that, the easiest way is through the Ollama WebUI.

Download and run the Ollama WebUI with the following command:

```bash
docker run -d --network=host \
            -e OLLAMA_BASE_URL=http://127.0.0.1:11434 \
            -v open-webui:/app/backend/data \
            --name open-webui --restart always \
            ghcr.io/open-webui/open-webui:main
```

That command starts **Open WebUI** in Docker and sets it up to talk with the local Ollama server we just started with the `ollama serve` command.

- `docker run -d` runs the container in the background (detached).

- `-network=host` puts the container on the host network, so it can reach services on your machine using `127.0.0.1` (localhost).

- `e OLLAMA_BASE_URL=http://127.0.0.1:11434` tells Open WebUI where your Ollama server is.

- `v open-webui:/app/backend/data` creates a persistent Docker volume so your Open WebUI chat history persists.

- `-name open-webui` names the container.

- `-restart always` makes it auto-start again after reboots or crashes.

- `ghcr.io/open-webui/open-webui:main` is the image being run (the `main` tag).

To see if it all worked well, run this command:

```bash
docker ps
```

If you see a container with the name `open-webui` and the status `Up`, you are good to go, and you can now safely visit: `http://localhost:8080` to view the WebUI.

### Step 4: Add Custom Tools for Function Calling

Once you're in, you should see the new model `ministral-3:3b-instruct-2512` in the list of models. Now, let's add our custom tools.

First, let's test it with local tools, which are smaller Python functions that the model can call.

Head over to the Workspace tab in the left sidebar, and in the Tools section, click on the "+ New Tool" button, and paste the following code: [Local Tools](https://gist.github.com/shricodev/422b04f2eac96c77a3210adaea1a1a9c)

Now, in a new chat, try saying something like:

> "What's 6 + 7?"

The model should use our added tool to answer the question.

### Step 5: Add Remote MCP Tools for Function Calling

But that's not fun. 😪 We want to use tools that are hosted remotely, right?

For that, we can use Composio MCP, which is well-maintained and supports over 500 apps, so why not?

Now we need the MCP URL... For that, head over to [Composio API Reference](https://docs.composio.dev/rest-api/tool-router/post-labs-tool-router-session?explorer=true).

Add your API key and the user ID, and make a request. You should get the MCP URL returned to you in a JSON format. Keep a note of the URL.

> 💡 But is this the only way? **No**, this is just a quick, hacky way I use to get the URL back without any coding.

Now, you're almost there. All you need to do is add a new MCP server with this URL.

Click on your profile icon at the top, under **Admin Panel**, click on the **Settings** tab, and under **External Tools**, click on the "+" button to add external servers.

In the dialog box, make sure that you switch to **MCP Streamable HTTP** from **OpenAPI**, and fill in the URL and give it a nice name and description.

For Authentication, check **None**; we will handle authentication with the additional header "x-api-key". In the Headers input, add the following JSON:

```json
{
  "x-api-key": "YOUR_COMPOSIO_API_KEY"
}
```

Once that's done, click on **Verify Connection**, and if everything went well, you should see "Connection Successful." That's pretty much all you need to do to use local and remote tools with the Ministral 3 3B model using Ollama.

The steps here are going to be pretty much the same for any other models that support tool calling.

Here's an example of the model returning a response after doing tool calls:

> 💡 **NOTE:** The model might take quite some time to answer the question and perform tool calls, and that pretty much depends on your system as well.

If you feel something is not working as expected, you can always view logs of your Ollama WebUI.

```bash
docker logs -f open-webui
```

## Next Steps

This entire demo was done using Ollama and Ollama WebUI. See if you can get it working with vLLM and Ollama WebUI. The steps are going to be quite similar.

Just follow the docs for vLLM installation based on your system, and follow the [guide](https://docs.vllm.ai/en/latest/getting_started/installation/) which should get you going.

Let me know if you are able to make it work.

## Conclusion

That's it. We just ran a lightweight, quantized Ministral 3 3B model in Ollama, wrapped it with Open WebUI, and showed it can perform real tool calling, both with small local Python tools and remote MCP tools via Composio.

You now have a simple local setup where the model can do more than just chat. The best part is, the steps won't change for other models, and you can quickly have your own local model that's entirely yours.

Now, try adding more toolkits and models (if your system can handle it) and just experiment. You already have a clear understanding of Ministral 3 3B and running models locally with Ollama. Apply it to your actual work, and you'll thank me later.

Well, that's all for now! I will see you in the next one. 🫡


