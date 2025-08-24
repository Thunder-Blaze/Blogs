---
title: Zafkiel — Building an Anime Client with Svelte, Tauri, and Rust
description: A developer’s journey building Zafkiel, a lightweight and customizable anime client with Svelte, Tweakcn, Tauri, and Rust.
slug: zafkiel
date: 2025-08-24 00:17:05+0530
image: cover.jpg
categories:
    - Development
    - Projects
tags:
    - Svelte
    - Rust
    - Tauri
    - AniList
    - TanStack Query
    - Shadcn
---

# Zafkiel — Building a Native Anime Client with Svelte, Tauri, and Rust

Over the past year, I’ve been working on a project very close to my heart: **Zafkiel**, a desktop anime client built with **Svelte, Tauri, and Rust**. The goal has always been simple yet ambitious — to create a fast, lightweight, and customizable app for browsing and tracking anime, one that feels **native, modern, and open to the community**.

Although Zafkiel is already in **production use**, it remains under active development and is not yet fully stable. Still, it’s at a stage where I can reflect on why I started it, the choices I made, and the direction it’s heading.

---

## Why Zafkiel?

Existing anime tracking platforms and apps are powerful, but I often found them lacking in either **performance, design flexibility, or openness**. Many desktop clients are simply Electron wrappers, which work but can feel heavy. I wanted something different:

* **Lightweight** — fast startup and minimal resource usage.
* **Beautiful** — a modern design with multiple themes.
* **Hackable** — easy to modify, extend, and contribute to.
* **Cross-platform** — consistent performance on Linux, Windows, and macOS.

That vision became **Zafkiel** — named after Kurumi Tokisaki’s angel in *Date A Live*, symbolizing control over time. It felt like the right metaphor for an app meant to give users **control over their anime experience**.

---

## Choosing the Stack

### Frontend: Svelte + Shadcn & Tweakcn

I chose **Svelte** for the frontend because of its **simplicity, reactivity, and developer experience**. Unlike heavier frameworks, Svelte compiles away much of the overhead, leaving a lean and efficient app. This makes UI development **faster and more intuitive**, letting me focus on design and user experience.

To bring flexibility to the design, I integrated **Shadcn** & **Tweakcn**, using which I enabled **multi-theme functionality**. With it, users can seamlessly switch between themes, creating a personalized experience without bloated complexity in the codebase.

### Backend: Rust + Tauri

For the backend, I went with **Rust**. Rust not only ensures **safety and performance**, but it also makes Zafkiel highly modular. The backend is structured in a way that keeps each part of the logic isolated, easy to read, and easy to extend.

This modularity has another major advantage: **contributing is accessible to others**. Developers can dive into specific parts of the codebase without needing to understand everything at once. Whether it’s improving caching, adding API integrations, or refining error handling, contributions are straightforward.

With **Tauri** acting as the bridge between the frontend and backend, the result is a **native desktop application** that consumes only a fraction of the resources of an Electron-based app, while maintaining strong security guarantees.

---

## **Current State**

At the moment, Zafkiel is still in **basic development**.  
Some core features are working, but it’s far from stable or feature-complete.  

Right now, I’m focusing on:  

- Building out the **core UI** with Svelte and Tailwind.
- Structuring the **Rust backend** for modularity and clean APIs (*although most of this is already done*).  
- Refining how I use **TanStack Query** for AniList API calls.  

It’s not something I can use daily yet, but the foundation is being laid carefully.  
My approach is **slow but steady** — making sure I get the architecture right before layering on more features.  

---

## **Looking Ahead**

The roadmap for Zafkiel includes:  

- **Stable AniList API integration** with efficient caching.  
- **Polished UI/UX** with multiple themes and smooth transitions.  
- **Cross-platform support** to ensure consistency on Linux, Windows, and macOS.  
- **Extensibility** so contributors can add new features or integrations easily.  

I’m not rushing this project — the goal is to **build it right, not just fast**.  

---

## **Conclusion**

Zafkiel is still a work in progress, but it represents my vision of what an anime client should be: **lightweight, customizable, and open for contributions**.  

With **Svelte** powering the UI, **Tweakcn** providing theming flexibility, **TanStack Query** handling data-fetching experiments, and a **modular Rust backend** ensuring performance and stability, Zafkiel is slowly but surely becoming the client I’ve always wanted.  

It’s not production-ready yet, but the journey has only just begun — and I’m excited to see where it goes from here.  

---
