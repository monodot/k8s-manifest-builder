<template>
  <h2 class="text-4xl font-bold text-gray-800 py-4">Example - Pod</h2>

  <div class="md:flex md:flex-wrap">
    <div class="md:w-1/2 md:pr-4">
      <!-- Use 'ref' to assign a reference ID to this component -->
      <div ref="jsoneditor"></div>

      <button v-on:click="writeLog">Submit (console.log)</button>
    </div>
    <div class="md:w-1/2 md:pr-4">
      <pre v-text="outputJson" class="font-mono border py-2 px-3 shadow bg-white"></pre>
    </div>
  </div>

</template>

<script>
import { JSONEditor } from '@json-editor/json-editor/dist/jsoneditor.js'

export default {
  name: 'Example',
  props: {
    msg: String
  },
  data() {
    return { 
      // Adding this object into our data structure...
      editor: null
    }
  },
  methods: {
    writeLog: function() {
      console.log(this.editor.getValue())
    }
  },
  computed: { },
  mounted() {
    // Taking inspiration from: https://github.com/Adam-Jimenez/vue2-jsoneditor/blob/master/src/components/JsonEditor.vue
    const container = this.$refs.jsoneditor

    // CONT: Add stuff to this JSON schema. See if it's a possibility to use to generate the form.
    this.editor = new JSONEditor(container, {
      schema: {
        "title": "Pod",
        "type": "object",
        "properties": {
          "apiVersion": {
            "type": "string",
            "options": {
              "hidden": "true"
            },
            "default": "v1"
          },
          "kind": {
            "type": "string",
            "options": {
              "hidden": "true"
            },
            "default": "Pod"
          },
          "metadata": {
            "type": "object",
            "properties": {
              "name": {
                "type": "string",
                "default": "my-pod-0"
              }
            }
          },
          "spec": {
            "type": "object",
            "properties": {
              "template": {
                "type": "object",
                "properties": {
                  "spec": {
                    "type": "object",
                    "properties": {
                      "containers": {
                        "type": "array",
                        "format": "table",
                        "title": "Containers",
                        "uniqueItems": true,
                        "items": {
                          "type": "object",
                          "title": "Container",
                          "properties": {
                            "name": {
                              "type": "string"
                            },
                            "image": {
                              "type": "string"
                            }
                          }
                        },
                        "default": [
                          {
                            "name": "container1",
                            "image": "hello-world:latest"
                          }
                        ]
                      }
                    }
                  }
                }
              }
            }
          },
        }
      },
      theme: 'tailwind'
    })

    this.editor.on('change', function() {
      var json = this.getValue()
      this.outputJson = JSON.stringify(json, null, 2)
    })
  }
}
</script>
