---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: true
# some information about the slides, markdown enabled
info: |
  ## CDC Sharing Slides
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS (experimental)
css: unocss

---

# Stream Your Data with Debezium

## Postgres to Postgres

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Get started <carbon:arrow-right class="inline"/>
  </span>
</div>

<!-- <div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div> -->

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: two-cols
---

# Hi! I am Chandra Wijaya

<p class="text-xl mb-6">By the power of Coffee bestowed upon me, I am a developer!</p>

- Father of two (gonna be) kids
- Team leader of
  - myCORE
  - Loan Support
  - Digital Loan 


<hr class="mt-4 mb-8 text-gray-300"/>


<logos-spring-icon class="mr-3" />
<logos-nextjs-icon class="mr-3" />
<logos-vue  class="mr-3"/>
<logos-angular-icon class="mr-3"/>
<logos-nodejs-icon class="mr-3"/>
<logos-tailwindcss-icon  class="mr-3"/>
<logos-bootstrap class="mr-3" />
<logos-flutter class="mr-3" />
<logos-nuxt-icon class="mr-3" />


<div class="abs-bl m-6 flex gap-2 space-x-4">
<div class="flex flex-col">
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-sm icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github /> @chandragie
  </a>
  <a href="https://instagram.com/chandragie" target="_blank" alt="Instagram"
    class="text-sm icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-instagram /> @chandragie
  </a>
  </div>
<div class="flex flex-col">
  <a href="https://twitter.com/chandragie" target="_blank" alt="Twitter"
    class="text-sm icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-twitter /> @chandragie
  </a>
  <a href="https://chandrawijaya.me" target="_blank" alt="Website"
    class="text-sm icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-http /> https://chandrawijaya.me
  </a>
  </div>
</div>

::right::

<img src="/me.png" class="w-full ml-1/8 -mt-1/8">

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Why are we here?

- Use cases of change data streams
- How to do it
- Demo

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# What is CDC?

<p class="text-xl">Get an event stream of data and schema changes in your DB</p>


<div class="w-full flex justify-center mt-10">
  <img src="/whatiscdc.png" class="w-200" />
</div>


<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# When do we need CDC?

<p class="text-xl">Data Replication</p>

- Replicate data to other DB
- Analytics system data feeding
- Cross team data feeding

<div class="w-full flex justify-center">
  <img src="/cdcusecases.png" class="w-150" />
</div>

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: two-cols
---

# When do we need CDC?

<p class="text-xl">Microservices Architecture</p>

- Propagate data between different services **without coupling**
- Each service keeps **optimised views locally**

::right::
<div class="w-full flex justify-center">
  <img src="/microservices.png" class="w-250 mt-20" />
</div>

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Why replicate?

- Event driven architecture
- Migration sync
- Data analytic purposes

<div class="w-full flex justify-center">
  <img src="/whyreplicate.png" class="w-160 ml-60 -mt-30" />
</div>

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: two-cols
---

# How do we capture changes?

<p class="text-xl">Possible Approaches:</p>

- Dual writes
  - Failure handling?
  - Prone to race conditions


- Batch job
  - Realtime changes?
  - What if the job errors?
  
<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

::right::

<img src="/dualwrites.png" class="w-full mt-20"/>

> <Link href="https://www.confluent.io/blog/using-logs-to-build-a-solid-data-infrastructure-or-why-dual-writes-are-a-bad-idea/"> Why dual writes are bad ideas</Link>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# How do we capture changes?

<p class="text-xl">Possible Approaches:</p>

- Polling
  - Performance hit
  - How to find changed rows?
  - How to find deleted rows?

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Database Logs

- DB records changes in log files, then updates tables
- Logs used for TX recovery, replication, etc.
  - MySQL : binlog
  - Postgres : write-ahead-log
  - MongoDB : op log

<p class="text-xl">Let's use that for CDC ~</p>

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Use Apache Kafka as Basis

- Messages have a key
- Guaranteed ordering
- Pull based

<div class="w-full flex justify-end">
  <img src="/kafka.png" class="w-80 mr-30" />
</div>

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Kafka Connect

- A framework for `source` and `sink` connectors
- Track offsets
- Schema supports
- Rich ecosystem of connectors

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# CDC Topology with Kafka Connect

<div class="w-full flex justify-center">
  <img src="/topology.png" class="w-300 mt-10" />
</div>

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# CDC Message Structure

- Key and Value
- Payload : **`before`** and **`after`**
- JSON
- Avro

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Debezium Connectors

- MySQL
- Postgres
- MongoDB
- Coming soon:
  - Oracle
  - SQL Server
  - Cassandra?
  - MariaDB?

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Message Transformations

Altering individual messages via SMTs

- Apply on `source` and `sink` side
- Use cases:
  - Extraction
  - Conversion
  - Routing

Provided by Debezium:

- Logical table router
- Event flattening SMT (take the `after` state)

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: center
class: "text-center"
---

<h1>Demo</h1>

---

# References

- [Gunnar Morling talks at Devoxx Belgium 2017 (Streaming Database Changes with Debezium)](https://www.youtube.com/watch?v=IOZ2Um6e430&t=2021s)
- [Debezium Github repository](https://github.com/debezium/debezium-examples/tree/main/unwrap-smt)
- [My Github repository](https://github.com/chandragie/cdc-debezium)

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Questions?

<div class="w-full flex justify-center">
<img src="/thanks.JPG" class="h-100" />
</div>

---

# What's next?

- AWS DevAx
- Dockerize your app!
- Troubleshoot elegantly with meaningful logging
- GraphQL now and thank me later!


Please vote what I should do next at:

<Link href="https://bit.ly/cwa-cop-survey"><p class="text-2xl">https://bit.ly/cwa-cop-survey</p></Link>

<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: center
class: text-center
---

<div class="text-3xl">

Orang tua laki-laki namanya Ayah

<p v-click>Harus kolaboratif dan supportif</p>

<p v-click=2>Sekian presentasi saya</p>

<p v-click=3>Semoga dapet kesan positif</p>

</div>
<div class="abs-b m-6 flex gap-2 justify-between opacity-50">
  <p class="text-sm">#Debezium #Kafka</p>
  <p class="text-sm">Stream Your Data with CDC</p>
</div>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
