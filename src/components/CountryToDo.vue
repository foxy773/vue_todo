<template>
    <li>
        <div class="view">
            <button :class="`complete ${done ? 'yes' : 'no'}`"></button>
            <label>{{ name }}</label>
            <button class="destroy"></button>
        </div>
    </li>
</template>

<script>

export default {
    props: [
        "name",
        "done"
    ]
}
</script>

<style scoped>
button {
 margin: 0;
 padding: 0;
 border: 0;
 background: none;
 font-size: 100%;
 vertical-align: baseline;
 font-family: inherit;
 color: inherit;
 -webkit-appearance: none;
 -ms-appearance: none;
 -o-appearance: none;
 appearance: none;
}

button,
input[type='checkbox'] {
 outline: none;
}

 li {
 position: relative;
 font-size: 24px;
 border-bottom: 1px dotted #ccc;
}

 li:last-child {
 border-bottom: none;
}

 li.editing {
 border-bottom: none;
 padding: 0;
}

 li.editing .edit {
 display: block;
 width: 506px;
 padding: 13px 17px 12px 17px;
 margin: 0 0 0 43px;
}

 li.editing .view {
 display: none;
}

 li label {
 white-space: pre;
 word-break: break-word;
 padding: 15px 60px 15px 15px;
 margin-left: 45px;
 display: block;
 line-height: 1.2;
 -webkit-transition: color 0.4s;
 transition: color 0.4s;
}

 li .destroy,
.complete {
 display: none;
 position: absolute;
 top: 0;
 right: 10px;
 bottom: 0;
 width: 40px;
 height: 40px;
 margin: auto 0;
 font-size: 22px;
 color: #a88a8a;
 -webkit-transition: all 0.2s;
 transition: all 0.2s;
}

 li .complete {
 left: 10px;
}

 li .destroy:hover,
.complete:hover {
 text-shadow: 0 0 1px #000, 0 0 10px rgba(199, 107, 107, 0.8);
 -webkit-transform: scale(1.3);
 -ms-transform: scale(1.3);
 transform: scale(1.3);
}

li .destroy:after {
 content: '✖';
}
 li .complete.yes:after {
 content: '✅️';
}
 li .complete.no:after {
 content: '☑️';
}

 li:hover .destroy,
.complete {
 display: block;
}

 li .edit {
 display: none;
}

 li.editing:last-child {
 margin-bottom: -1px;
}
</style>