<style lang="scss">
    header {
      width: 100%;
      background: white;
      margin: 0;
      padding:1rem;
      padding-top:0;

      .wrapper {
        display: grid;
        grid-template-columns: auto auto;
        align-items: center;
        justify-content: space-between;
        border-bottom: 1px solid #ddd;

        nav {
          font-size:0;
          
          a {
            color: black;
            text-decoration: none;
            font-weight: 500;
            font-family: "Open Sans";
            margin-left: 3.2rem;
            text-transform: uppercase;
            font-size: 1.2rem;

            @media screen and (max-width: 600px) {
                margin-left:1.5rem;
                font-size:0.9rem;
            }

            &:hover {
                color: red;
            }
          }
        }
      }
    }

    footer {
      background: white;
      padding: 1rem;
      padding-bottom:1rem;

      .wrapper {
        display: grid;
        grid-template-columns: auto auto auto;
        grid-column-gap: 5rem;
        justify-content: space-around;
        border-top:1px solid #ddd;

        .column {
          h2 {
            color: black;
            font-family: "Open Sans";
            text-transform: uppercase;
            margin-bottom: 1rem;
            font-size: 1.2rem;
          }
          a {
            display: block;
            width: max-content;
            margin-bottom: 1rem;
            color: #aaa;
            font-family: "Open Sans";
            font-size: 1.2rem;
          }
        }
      }
    }
</style>

<script>
    import Logo from '$lib/logo.svelte';
</script>

<header>
  <div class="wrapper">
    <Logo />
    <nav>
      <a href="/signin">Sign In</a>
      <a href="/register">Register</a>
    </nav>
  </div>
</header>

<slot></slot>

<footer>
  <div class="wrapper">
    <div class="column">
      <h2>Site</h2>
      <a href="/about">About</a>
      <a href="/models">Models</a>
      <a href="/notebooks">Notebooks</a>
      <a href="/research">Research</a>
    </div>
    <div class="column">
      <h2>Links</h2>
      <a href="https://arxiv-sanity-lite.com">Arxiv Sanity</a>
      <a href="https://paperswithcode.com">Papers With Code</a>
      <a href="https://midjourney.com">Midjourney</a>
      <a href="https://openai.com">OpenAI</a>
    </div>
    <div class="column">
      <h2>Self</h2>
      <a href="https://github.com/tjwhitaker">Github</a>
      <a href="https://scholar.google.com/citations?user=Rb-16TYAAAAJ&hl=en">Scholar</a>
      <a href="https://orcid.org/0000-0003-3792-3901">Orcid</a>
    </div>
  </div>
</footer>