<script>
  let tabsandiframes = [];

  let nextid = 1;

  function isUrl(val = "") {
    if (
      /^http(s?):\/\//.test(val) ||
      (val.includes(".") && val.substr(0, 1) !== " ")
    )
      return true;
    return false;
  }

  function go(value) {
    let url = value.trim();
    if (!isUrl(url)) url = "https://www.neeva.com/search?q=" + url;
    else if (!(url.startsWith("https://") || url.startsWith("http://")))
      url = "https://" + url;
    let activeIframe = document.querySelector("iframe.active");

    activeIframe.style.display = "block";
    activeIframe.src = "/go/" + url;
  }

  let tabs = [];
  let iframes = [];
</script>

<div id="topbar">
  <div id="tabsandmore">
    <div id="tabs">
      {#each tabsandiframes as tabandiframe, index (tabandiframe)}
        <button
          class="tab"
          id={"tab" + tabandiframe}
          on:click={() => {
            let tab = document.querySelector("#tab" + tabandiframe);
            let iframe = document.getElementById(tabandiframe);
            tabs.forEach((elmnt) => {
              elmnt.classList = "tab";
            });
            iframes.forEach((elmnt) => {
              elmnt.classList = "";
              elmnt.style.display = "none";
            });
            tab.classList.add("active");
            iframe.classList.add("active");

            if (iframe.src !== "") {
              iframe.style.display = "block";
            } else {
              iframe.style.display = "none";
            }
          }}
          bind:this={tabs[index]}
        >
          <img
            class="favicon"
            alt="new tab icon"
            src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAABEElEQVR4nO2ZwWrCQBRFj0KC/oe/2NpV0U9w1ZX+Qb+i3Xdf/ZFK4ElgCsNAMDOaPIP3wNvlJfdyJ1ncgBBCiIFYAlvgBDSAjTxNePYmaMkW/+0g2jqm1bLIMbB9ANGWzHuOgVO0+AJUV65fAZ/JA/fAnHIq4DW632/Ocnzm65478yD6nibq5J3oTSwihxnwkewfbjRhJVpKDXQlUTJn4MfDQFcS95jRDPwnsQP+pmrgVqrkKzQ5A+lXaJIGWmTAG9MRcsaUgDOmBJwxJeCMKQFnTAk4Y0rAGXvKBJqCYmsI6tJiK64W104mauCttFrcDNDpjFruLh+sXv/KrdcJC63ro+MPjmPQkC1eCCEEfbgAuJ68dbQb+kgAAAAASUVORK5CYII="
          />
          <p>New tab</p>
        </button>
      {/each}
    </div>
    <div id="settingsandnewtab">
      <button id="settingsbutton">
        <i class="fa-solid fa-gear" />
      </button>
      <button
        id="newtabbutton"
        on:click={() => {
          let newnextid = nextid;
          let newtabsandiframes = [...tabsandiframes, newnextid];
          tabsandiframes = newtabsandiframes;
          nextid = nextid + 1;
          function checkIfTabExists() {
            if (document.querySelector("#tab" + newnextid) == null) {
              window.setTimeout(checkIfTabExists, 50);
            } else {
              document.querySelector("#tab" + newnextid).click();
              go("neeva.com");
            }
          }
          checkIfTabExists();
        }}
      >
        <i class="fa-solid fa-plus" />
      </button>
    </div>
  </div>
  <form id="urlbar">
    <input type="text" />
  </form>
</div>

{#each tabsandiframes as tabandiframe, index (tabandiframe)}
  <iframe id={tabandiframe} title="iframe" src="" bind:this={iframes[index]} />
{/each}

<svelte:window
  on:load={() => {
    document.querySelector("#newtabbutton").click();
  }}
/>
