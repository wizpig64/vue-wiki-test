<template>
  <div>
    <tiptap-editor ref="editor"
                   :doc="doc"
                   :editable="!history"
                   :extensions="tiptap_extensions"
                   @update="editor_update">
      <editor-menubar slot="menubar"
                      slot-scope="{ nodes, marks, focus}"
                      :nodes="nodes" :marks="marks" :focus="focus"/>
      <div slot="content" slot-scope="props"></div>
    </tiptap-editor>
    <p>
      <span class="btn-group" role="group">
        <button type="button" class="btn btn-sm btn-secondary"
                v-if="!history && unsaved_changes" @click="cancel_edits">
          Cancel
        </button>
        <button type="button" class="btn btn-sm btn-primary"
                v-if="!history && unsaved_changes" @click="save">
          Save
        </button>
        <button type="button" class="btn btn-sm btn-secondary"
                v-if="history" @click="cancel_history">
          Cancel
        </button>
        <button type="button" class="btn btn-sm btn-warning"
                v-if="history" @click="revert">
          Revert
        </button>
      </span>
    </p>
    <pre v-if="debug"><code>{{document}}</code></pre>
  </div>
</template>

<script>
import { Editor } from 'tiptap'
import {
  BlockquoteNode,
  BoldMark,
  BulletListNode,
  CodeBlockNode,
  CodeMark,
  HardBreakNode,
  HeadingNode,
  HistoryExtension,
  ItalicMark,
  LinkMark,
  ListItemNode,
  OrderedListNode,
  StrikeMark,
  TodoItemNode,
  TodoListNode,
  UnderlineMark
} from 'tiptap-extensions'
import diff from 'deep-diff'
import EditorMenubar from './EditorMenubar'

export default {
  name: 'Editor',
  data () {
    return {
      debug: false,
      error: null,
      tiptap_extensions: [
        new BlockquoteNode(),
        new BoldMark(),
        new BulletListNode(),
        new CodeBlockNode(),
        new CodeMark(),
        new HardBreakNode(),
        new HeadingNode({ maxLevel: 3 }),
        new HistoryExtension(),
        new ItalicMark(),
        new LinkMark(),
        new ListItemNode(),
        new OrderedListNode(),
        new StrikeMark(),
        new TodoItemNode(),
        new TodoListNode(),
        new UnderlineMark()
      ],
      document_buffer: null,
      document_edits: null
    }
  },
  components: {
    'tiptap-editor': Editor,
    EditorMenubar
  },
  props: {
    document: Object,
    history: Object
  },
  methods: {
    editor_update ({ getJSON }) {
      let edits = getJSON()
      if (!this.history) {
        this.document_edits = edits
      }
    },
    cancel_edits () {
      this.document_buffer = this.document
      this.document_edits = null
      this.$refs.editor.setContent(this.document)
    },
    cancel_history () {
      this.$emit('select_history', null)
    },
    save () {
      if (this.unsaved_changes) {
        this.$emit('save', this.document_edits)
      }
      this.document_edits = null
    },
    revert () {
      if (this.history) {
        this.$emit('revert', this.history.id)
      }
    }
  },
  computed: {
    unsaved_changes () {
      return this.document_edits && diff(this.document, this.document_edits)
    },
    doc () {
      if (this.history) {
        return this.history.document
      } else {
        return this.document_buffer || this.document
      }
    }
  },
  created () {
    this.document_buffer = this.document
  },
  watch: {
    history () {
      // save to document_buffer only when switching history.
      // if we save to it while the user is typing, it will reset the cursor on every key.
      this.document_buffer = this.document_edits
    }
  }
}
</script>
