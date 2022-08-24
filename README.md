Experimenting with mermaid to document the flow for how the release process and nodejs.org currently works.
```mermaid
flowchart TD
    subgraph user[Releaser]
    start([Start])-->A(Start release build)--> promote(promote)-->blog(create blog post)-->F([End])
    end
    subgraph jenkins[Release CI]
    builds
    end
    subgraph github[GitHub]
    gh[nodejs/nodejs.org]
    end
    subgraph www[www server]
      staging[(staging)] --> promotion -->dist[(dist)]
      dist-->rebuildIdx(rebuild index)
      
      subgraph chkIndex[Check index]
      idxChanged{Index Changed}--No-->idxChanged
      end
      
      rebuildIdx-.->idxChanged
      webhook-->rebuild
      idxChanged--Yes-->rebuild(Rebuild website)
      rebuild-->cdnq(queue cdn purge)
      cdnq-->purge(cdn purge)
    end
    A-->builds(release builds)
    gh-.->webhook
    promote-->promotion
    blog-->gh
    builds-->staging
    purge-->cloudflare

```

Reference image
![Original reference flow](https://raw.githubusercontent.com/richardlau/nodejs.org-flow/main/release%20process.svg)
