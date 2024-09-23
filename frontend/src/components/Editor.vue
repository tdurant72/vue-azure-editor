<template>
    <div>
        <section v-if="editor" class="buttons flex items-center flex-wrap gap-x-4 border border-gray-400 p-4">
            <button @click="editor.chain().focus().toggleBold().run()"
                :disabled="!editor.can().chain().focus().toggleBold().run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('bold') }">
                <BoldIcon title="Bold" />
            </button>
            <button @click="editor.chain().focus().toggleItalic().run()"
                :disabled="!editor.can().chain().focus().toggleItalic().run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('italic') }">
                <ItalicIcon title="Italic" />
            </button>
            <button @click="editor.chain().focus().toggleStrike().run()"
                :disabled="!editor.can().chain().focus().toggleStrike().run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('strike') }">
                <StrikeIcon title="Strike Through" />
            </button>
            <button @click="editor.chain().focus().toggleCode().run()"
                :disabled="!editor.can().chain().focus().toggleCode().run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('code') }">
                <CodeIcon title="Code" />
            </button>
            <button @click="editor.chain().focus().setParagraph().run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('paragraph') }">
                <ParagraphIcon title="Paragraph" />
            </button>
            <button @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('heading', { level: 1 }) }">
                <H1Icon title="H1" />
            </button>
            <button @click="editor.chain().focus().toggleHeading({ level: 2 }).run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('heading', { level: 2 }) }">
                <H2Icon title="H2" />
            </button>
            <button @click="editor.chain().focus().toggleHeading({ level: 3 }).run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('heading', { level: 3 }) }">
                <H3Icon title="H3" />
            </button>
            <button @click="editor.chain().focus().toggleHeading({ level: 4 }).run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('heading', { level: 4 }) }">
                <H4Icon title="H4" />
            </button>
            <button @click="editor.chain().focus().toggleHeading({ level: 5 }).run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('heading', { level: 5 }) }">
                <H5Icon title="H5" />
            </button>
            <button @click="editor.chain().focus().toggleBulletList().run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('bulletList') }">
                <BulletListIcon title="Bulleted List" />
            </button>
            <button @click="editor.chain().focus().toggleOrderedList().run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('orderedList') }">
                <NumberedListIcon title="Ordered List" />
            </button>

            <button @click="editor.chain().focus().toggleBlockquote().run()"
                :class="{ 'bg-gray-200 rounded border ': editor.isActive('blockquote') }">
                <BlockQuotetIcon title="Block Quote" />
            </button>
            <button @click="editor.chain().focus().setHorizontalRule().run()">
                <HorizontalIcon title="Horizontal rule" />
            </button>
            <button @click="editor.chain().focus().setHardBreak().run()">
                <HardBreakIcon title="Hard break" />
            </button>
            <button @click="setLink()">
                <LinkIcon title="Add Link" />
            </button>
            <button @click="addImage()">
                <ImageIcon title="Add Image" />
            </button>
            <button @click="editor.chain().focus().undo().run()" :disabled="!editor.can().chain().focus().undo().run()">
                <UndoIcon title="Undo" />
            </button>
            <button @click="editor.chain().focus().redo().run()" :disabled="!editor.can().chain().focus().redo().run()">
                <RedoIcon title="Redo" />
            </button>

        </section>
        <editor-content :editor="editor" />
    </div>
</template>

<script setup>
import { ref, watchEffect, onUpdated, onBeforeUnmount } from 'vue';
import BoldIcon from 'vue-material-design-icons/FormatBold.vue';
import ItalicIcon from 'vue-material-design-icons/FormatItalic.vue'
import StrikeIcon from 'vue-material-design-icons/FormatStrikethrough.vue'
import CodeIcon from 'vue-material-design-icons/CodeTags.vue'
import ParagraphIcon from 'vue-material-design-icons/FormatParagraph.vue'
import H1Icon from 'vue-material-design-icons/FormatHeader1.vue'
import H2Icon from 'vue-material-design-icons/FormatHeader2.vue'
import H3Icon from 'vue-material-design-icons/FormatHeader3.vue'
import H4Icon from 'vue-material-design-icons/FormatHeader4.vue'
import H5Icon from 'vue-material-design-icons/FormatHeader5.vue'
import BulletListIcon from 'vue-material-design-icons/FormatListBulleted.vue'
import NumberedListIcon from 'vue-material-design-icons/FormatListNumbered.vue'
import BlockQuotetIcon from 'vue-material-design-icons/FormatQuoteOpen.vue'
import RedoIcon from 'vue-material-design-icons/Redo.vue'
import UndoIcon from 'vue-material-design-icons/Undo.vue'
import HorizontalIcon from 'vue-material-design-icons/Minus.vue'
import HardBreakIcon from 'vue-material-design-icons/FormatPageBreak.vue'
import LinkIcon from 'vue-material-design-icons/LinkVariant.vue'
import ImageIcon from 'vue-material-design-icons/ImageArea.vue'

import Link from '@tiptap/extension-link';
import { useEditor, EditorContent } from '@tiptap/vue-3'
import StarterKit from '@tiptap/starter-kit'
const props = defineProps({
    modelValue: String,
});
const emits = defineEmits(['update:modelValue'])


const editor = useEditor({
    editorProps: {
        attributes: {
            class: 'prose prose-sm sm:prose lg:prose-lg xl:prose-2xl text-left focus:outline-none border-t border-l border-r border-gray-400 min-h-[400px] max-h-[400px] overflow-y-auto p-4',
        },
        transformPastedText(text) {
            return text.toUpperCase()
        },
    },
    content: props.modelValue,
    extensions: [StarterKit, Link.configure({
        openOnClick: false,
        defaultProtocol: 'https',
    }),],
    onUpdate: ({ editor }) => {
        console.log(editor.getHTML());
        let content = editor.getHTML()
        emits('update:modelValue', content);
    },
})

const setLink=()=>{
      const previousUrl = editor.getAttributes('link').href
      const url = window.prompt('URL', previousUrl)
      // cancelled
      if (url === null) {
        return
      }
      // empty
      if (url === '') {
        editor
          .chain()
          .focus()
          .extendMarkRange('link')
          .unsetLink()
          .run()
        return
      }
      // update link
      editor
        .chain()
        .focus()
        .extendMarkRange('link')
        .setLink({ href: url })
        .run()
}
const addImage =()=>{
    const url = window.prompt('URL')
      if (url) {
        editor.chain().focus().setImage({ src: url }).run()
      }
}
onBeforeUnmount(() => {
    editor.destroy();
})

//     watchEffect(() => props.modelValue, (newValue, oldValue) => {
//     const isSame = newValue === oldValue;
//     if (isSame) {
//         return;
//     }

//     editor.value?.commands.setContent(newValue, false)
// });
</script>
