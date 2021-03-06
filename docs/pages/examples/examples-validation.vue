<template lang="html">
  <div class="examples-validation">
    <section>
      <h1>Validation</h1>
      <p>
        To validate tags, autocomplete items or the user input, a validation array
        can be passed to the tags input component. In this example, a tag has to be
        at least 8 characters long, can't contain a number or a brace and must not start
        with the string "Cannot".
      </p>
      <vue-tags-input
        v-model="tag"
        :tags="tags"
        :validation="validation"
        :autocomplete-items="filteredItems"
        @tags-changed="newTags => tags = newTags">
      </vue-tags-input>
      <div class="data">
        <span>tag</span>
        <el-code :code="tag"></el-code>
        <span>tags</span>
        <el-code :code="tags"></el-code>
      </div>
      <p>
        Each item in the validation array must contain the properties
        <span class="code">type</span> and <span class="code">rule</span>.
        <span class="code">type</span> will be added as a css class, if the appropriated
        <span class="code">rule</span> match a tag, the user input or an autocomplete item.
        The rule can by type of RegExp or function. In chapter <router-link :to="{ path: '/examples/styling'}">Styling</router-link>
        we will see how to use these css classes in detail.
      </p>
      <p>
        If the rule is valid, the class "valid", or if not, "invalid" is also added.
        If the tag input component find a duplicate item, the class "duplicate" is appended.
        By default the prop <span class="code">avoid-adding-duplicates</span> is true.
        So in this example it is impossible to add duplicates.
      </p>
      <p>
        If a validation item holds the property <span class="code">disableAdd: true</span>,
        a tag, which does match the appropriated rule, won't get just some css classes.
        In this case, the tag can't be added to the tags array.
        Like every tag which starts with "Cannot" in this example.
      </p>
      <el-code class="html">
        <code>
&lt;vue-tags-input
  v-model=&quot;tag&quot;
  :tags=&quot;tags&quot;
  :validation=&quot;validation&quot;
  :autocomplete-items=&quot;filteredItems&quot;
  @tags-changed=&quot;newTags =&gt; tags = newTags&quot;&gt;
&lt;/vue-tags-input&gt;
        </code>
      </el-code>
      <el-code class="javascript">
        <code>
/* Other stuff like import tagsinput ... */

data() {
  return {
    tag: '',
    tags: [],
    autocompleteItems: [{
      text: 'Invalid because of "8"',
    }, {
      text: 'toShort',
    }, {
      text: 'I am valid',
    }, {
      text: 'Cannot be added',
    }, {
      text: 'Invalid cause of "{"',
    }],
    validation: [{
      type: 'min-length',
      rule: /^.{8,}$/,
    }, {
      type: 'no-numbers',
      rule: /^([^0-9]*)$/,
    }, {
      type: 'avoid-item',
      rule: /^(?!Cannot).*$/,
      disableAdd: true,
    }, {
      type: 'no-braces',
      rule(text) {
        return text.indexOf('{') !== -1 || text.indexOf('}') !== -1;
      },
    }],
  };
},

/* Computed properties, methods and more ... */
        </code>
      </el-code>
    </section>
  </div>
</template>

<script>
import VueTagsInput from '../../../vue-tags-input/vue-tags-input';
import ElCode from '../../components/el-code';

export default {
  name: 'examplesValidation',
  components: {
    VueTagsInput,
    ElCode,
  },
  data() {
    return {
      tag: '',
      tags: [],
      autocompleteItems: [{
        text: 'Invalid because of "8"',
      }, {
        text: 'toShort',
      }, {
        text: 'I am valid',
      }, {
        text: 'Cannot be added',
      }, {
        text: 'Invalid cause of "{"',
      }],
      validation: [{
        type: 'min-length',
        rule: /^.{8,}$/,
      }, {
        type: 'no-numbers',
        rule: '^([^0-9]*)$',
      }, {
        type: 'avoid-item',
        rule: /^(?!Cannot).*$/,
        disableAdd: true,
      }, {
        type: 'no-braces',
        rule(text) {
          return text.indexOf('{') !== -1 || text.indexOf('}') !== -1;
        },
      }],
    };
  },
  computed: {
    filteredItems() {
      return this.autocompleteItems.filter(i => new RegExp(this.tag, 'i').test(i.text));
    },
  },
};
</script>

<style lang="scss" scoped>
span, p {
  line-height: 1.5em;
  max-width: 900px;
}

.data {
  margin-top: 20px;
}
</style>
