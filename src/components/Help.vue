<template>
  <div class="is-help">
        <h1 class="is-help-header is-help-header-headers">
            Headers:
        </h1>
        <span>
            # Large Headline: 
        </span>
        <br>
        <div class="is-help-preview">
            <p>
                Preview:
            </p>
            <h1 class="is-markdown-help-element">
                Large Headline
            </h1>
        </div>

        <span>
            ## Medium Headline: 
        </span>
        <div class="is-help-preview">
            <p>
                Preview:
            </p>
            <h2 class="is-markdown-help-element">
                Medium Headline
            </h2>
        </div>

        ### Small Headline: 

        <div class="is-help-preview">
            <p>
                Preview:
            </p>
            <h3 class="is-markdown-help-element">
                Small Headline
            </h3>
        </div>

        <hr class="is-help-divider">

        <h1 class="is-help-header"> 
            Lists:
        </h1>

        <h2>
            Unordered List:
        </h2>
        * List Item 1
        <br>
        * List Item 2
        <br>
        * List Item 3

        <div class="is-help-preview">
            <p>
                Preview:
            </p>
            <ul>
                <li>
                    List Item 1
                </li>
                <li>
                    List Item 2
                </li>
                <li>
                    List Item 3
                </li>
            </ul>
        </div>
        <h2>
            Ordered List:
        </h2>
        1. List Item 1
        <br>
        2. List Item 2
        <br>
        3. List Item 3

        <div class="is-help-preview">
            <p>
                Preview:
            </p>
            <ol>
                <li>
                    List Item 1
                </li>
                <li>
                    List Item 2
                </li>
                <li>
                    List Item 3
                </li>
            </ol>
        </div>

        <hr class="is-help-divider">

        <h1 class="is-help-header">
            Tables:
        </h1>
        | Item | Description| Price |
        <br>
        |-|-|-|
        <br>
        | column 1 | column 2 | column 3|
        <br>
        | column 1 | column 2 | column 3|
        <br>

        <div class="is-help-preview">
            <p>
                Preview:
            </p>
            <table class="mt-4">
                <thead>
                    <tr>
                        <th>
                            Item
                        </th>
                        <th>
                            Description
                        </th>
                        <th>
                            Price
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>
                            Column 1
                        </td>
                        <td>
                            Column 2
                        </td>
                        <td>
                            Column 3
                        </td>
                    </tr>
                    <tr>
                        <td>
                            Column 1
                        </td>
                        <td>
                            Column 2
                        </td>
                        <td>
                            Column 3
                        </td>
                    </tr>
                </tbody>
            </table>
            <br>
            <hr class="is-help-divider">

            <h1 class="is-help-header"> 
                Blockquotes:
            </h1>
            > This is a blockquote
            <p>
                Preview:
            </p>
            <blockquote class="mb-7">
                This is a blockquote
            </blockquote>

        </div>

        <div class="is-center">
            <button @click="home" class="button home">
                Home
            </button>
        </div>
  </div>
</template>

<script>
export default {
    methods: {
        home () {
            this.$router.push('/')
        }
    }
}
</script>