# JP-LRN-003-create-llama-nextjs
https://www.npmjs.com/package/create-llama

Steps
 -      npx create-llama@latest // run the example project template builder (choose nextjs instead of fastapi)
 -      npm install // install the modules and packages
 -      run wsl (windows shell for linux) since the generate script won't work in a regular cmd shell. You only need to do this once.
 -      npm run generate // will convert any files in the ./data/ folder into embeddings and load into vector store. You only need to do this once, or anytime you add new files to the /data folder
 -      npm run dev  // run the dev server
 -      http://localhost:3000/     // dev server should be up and running


Deployed to Vercel:
 - https://jp-lrn-003-create-llama-nextjs.vercel.app/
 - Note: you have to upload the .env file to create the required env variables needed by the app. 


# -------------------------------------------------------------------------------

```
Route (app)                              Size     First Load JS
┌ ○ /                                    525 kB          630 kB
├ ○ /_not-found                          986 B           107 kB
├ ƒ /api/chat                            151 B           106 kB
├ ƒ /api/chat/config                     151 B           106 kB
├ ƒ /api/chat/config/llamacloud          151 B           106 kB
├ ƒ /api/chat/upload                     151 B           106 kB
├ ƒ /api/files/[...slug]                 151 B           106 kB
└ ƒ /api/sandbox                         151 B           106 kB
+ First Load JS shared by all            106 kB
  ├ chunks/4bd1b696-d81b40d3908bb6c3.js  53 kB
  ├ chunks/517-581dde426bb2d8c2.js       50.6 kB
  └ other shared chunks (total)          2.01 kB
```

## code flow

load ui:
 - /app/page.tsx
 - ChatSection > /app/components/chat-section.tsx; post will call api: /api/chat

send request to api:
 - POST http://localhost:3000/api/chat

```
{
  "id": "ENcNQJuiW8Ah1yUS",
  "messages": [
    {
      "role": "user",
      "content": "what is the letter size?",
      "annotations": []
    }
  ]
}
```

Server side:
 - /app/api/chat/route.ts > export async function POST()
 - retrieve document ids from the annotations of all messages (if any)
 - create chat engine with index using the document ids
 - retrieve user message content from Vercel/AI format
 - Setup callbacks
 - Calling LlamaIndex's ChatEngine to get a streamed response
 - chatHistory.push
 - return streamed data response

example response:

```
[{"type":"events","data":{"title":"Using tool: 'query_engine' with inputs: 'query: letter size'"}}]

[{"type":"sources","data":{"nodes":[{"metadata":{"page_number":1,"total_pages":8,"file_path":"L:\\_github\\_JP_github\\JP-LRN-003-create-llama-nextjs\\data\\101.pdf","file_name":"101.pdf","private":"false"},"id":"7ca1e3c7-1504-45d0-9541-0d4ca5e80440","score":0.5101614071113206,"url":"http://localhost:3000/api/files/data/101.pdf","text":"Domestic Mail Manual • Updated 7-9-23\n101101.1.2\nRetail Mail: Physical Standards for Letters, Cards, Flats, and Parcels\n101 Physical Standards\n1.0 Physical Standards for Letters\n1.1 Dimensional Standards for Letters\nLetter-size mail is the following:\na.Not less than 5 inches long, 3-1/2 inches high, and 0.007-inch thick.For\npieces more than 6 inches long or 4-1/4 inches high, the minimum thickness\nis 0.009.(Pieces not meeting the 0.009 thickness are subject to a\nnonmachinable surcharge under 1.2f.)b.Not more than 11-1/2 inches long, or more than 6-1/8 inches high, or more\nthan 1/4-inch thick.c.Not more than 3.5 ounces.(Charge flat-size prices for First-Class Mail\nletter-size pieces over 3.5 ounces.)d.Rectangular, with four square corners and parallel opposite sides.Letter-size, card-type mailpieces made of cardstock may have finished\ncorners that do not exceed a radius of 0.125 inch (1/8 inch).See\nExhibit 201.1.1.1.1.2 Nonmachinable Criteria\nA letter-size piece is nonmachinable if it has one or more of the following\ncharacteristics (see 601.1.1.2 to determine the length, height, top, and bottom of\na mailpiece):\na.Has an aspect ratio (length divided by height) of less than 1.3 or more than 2.5.b.Is polybagged, polywrapped, enclosed in any plastic material, or has an\nexterior surface made of a material that is not paper.Windows in envelopes\nmade of paper do not make mailpieces nonmachinable.Attachments\nallowable under applicable eligibility standards do not make mailpieces\nnonmachinable.c.Has clasps, strings, buttons, or similar closure devices."},{"metadata":{"page_number":4,"total_pages":8,"file_path":"L:\\_github\\_JP_github\\JP-LRN-003-create-llama-nextjs\\data\\101.pdf","file_name":"101.pdf","private":"false"},"id":"46b6a4b9-2968-4aec-a699-72898de356b4","score":0.4977104834856645,"url":"http://localhost:3000/api/files/data/101.pdf","text":"Ineligible cards are subject to letter-size pricing.6.2.2 Postcard Dimensions\nEach card and part of a double card claimed at card pricing must be the\nfollowing:\na.Rectangular.b.Not less than 3-1/2 inches high, 5 inches long, and 0.007 inch thick.c.Not more than 4-1/4 inches high, or more than 6 inches long, or greater than\n0.016 inch thick.d.Not more than 3.5 ounces (Charge flat-size prices for First-Class Mail\ncard-type pieces over 3.5 ounces.)"}]}}]

[{"type":"events","data":{"title":"Retrieving context for query: 'letter size'"}}]

[{"type":"events","data":{"title":"Retrieved 2 sources to use as context for the query"}}]

[{"type":"tools","data":{"toolCall":{"id":"call_1GrNbe5yNJLS2bHWHml3KoAR","name":"query_engine","input":{"query":"letter size"}},"toolOutput":{"output":"Letter-size mail is defined as follows:\n\n1. **Dimensional Standards**:\n   - Minimum dimensions: Not less than 5 inches long, 3-1/2 inches high, and 0.007-inch thick. For pieces more than 6 inches long or 4-1/4 inches high, the minimum thickness is 0.009 inches.\n   - Maximum dimensions: Not more than 11-1/2 inches long, 6-1/8 inches high, or 1/4-inch thick.\n   - Weight: Not more than 3.5 ounces (pieces over 3.5 ounces are charged flat-size prices for First-Class Mail).\n\n2. **Shape**:\n   - Must be rectangular, with four square corners and parallel opposite sides. Card-type mailpieces made of cardstock may have finished corners with a radius not exceeding 0.125 inch (1/8 inch).\n\n3. **Nonmachinable Criteria**:\n   - A letter-size piece is considered nonmachinable if it has one or more of the following characteristics:\n     - An aspect ratio (length divided by height) of less than 1.3 or more than 2.5.\n     - It is polybagged, polywrapped, enclosed in plastic, or has a non-paper exterior surface.\n     - It has clasps, strings, buttons, or similar closure devices. \n\nThese standards ensure that letter-size mail can be processed efficiently through postal systems.","isError":false}}}]

[{"type":"suggested_questions","data":["What are the mailing options for letter-size mail?","How do I determine the postage for a letter-size piece?","Are there any restrictions on the content of letter-size mail?"]}]

```


# -------------------------------------------------------------------------------
# Readme file generated from template

This is a [LlamaIndex](https://www.llamaindex.ai/) project using [Next.js](https://nextjs.org/) bootstrapped with [`create-llama`](https://github.com/run-llama/LlamaIndexTS/tree/main/packages/create-llama).

## Getting Started

First, install the dependencies:

```
npm install
```

Second, generate the embeddings of the documents in the `./data` directory:

```
npm run generate
```

Third, run the development server:

```
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/basic-features/font-optimization) to automatically optimize and load Inter, a custom Google Font.

## Using Docker

1. Build an image for the Next.js app:

```
docker build -t <your_app_image_name> .
```

2. Generate embeddings:

Parse the data and generate the vector embeddings if the `./data` folder exists - otherwise, skip this step:

```
docker run \
  --rm \
  -v $(pwd)/.env:/app/.env \ # Use ENV variables and configuration from your file-system
  -v $(pwd)/config:/app/config \
  -v $(pwd)/data:/app/data \
  -v $(pwd)/cache:/app/cache \ # Use your file system to store the vector database
  <your_app_image_name> \
  npm run generate
```

3. Start the app:

```
docker run \
  --rm \
  -v $(pwd)/.env:/app/.env \ # Use ENV variables and configuration from your file-system
  -v $(pwd)/config:/app/config \
  -v $(pwd)/cache:/app/cache \ # Use your file system to store gea vector database
  -p 3000:3000 \
  <your_app_image_name>
```

## Learn More

To learn more about LlamaIndex, take a look at the following resources:

- [LlamaIndex Documentation](https://docs.llamaindex.ai) - learn about LlamaIndex (Python features).
- [LlamaIndexTS Documentation](https://ts.llamaindex.ai) - learn about LlamaIndex (Typescript features).

You can check out [the LlamaIndexTS GitHub repository](https://github.com/run-llama/LlamaIndexTS) - your feedback and contributions are welcome!


