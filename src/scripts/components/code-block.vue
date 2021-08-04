<template>
    <span class="codeblock">
        <code v-if="inline" v-highlight :class='(lang)? lang : "autodetect"'><slot></slot></code>
        <pre v-else><code v-highlight :class='(lang)? lang : "autodetect"'><slot></slot></code></pre>
    </span>
</template>

<script>
    export default {
        name: "CodeBlock",
        props: ['lang', 'inline'],
    }
</script>