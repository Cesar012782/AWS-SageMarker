# crowd\-tab<a name="sms-ui-template-crowd-tab"></a>

A component styled to look like a tab with information below\.

See an interactive example of an HTML template that uses this Crowd HTML Element in [CodePen](https://codepen.io/sagemaker_crowd_html_elements/pen/dyNPBGW)\.

The following is an example template that uses the `<crowd-tab>` element\. Copy the following code and save it in a file with the extension `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-tabs>
    <crowd-tab header="Tab 1">
      <h2>Image</h2>

      <img
        src="https://images.unsplash.com/photo-1478382188900-5bb598fe27d3?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1351&q=80"
        style="max-width: 40%"
      >

      <h2>Text</h2>
      <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </p>
      <p>
      Sed risus ultricies tristique nulla aliquet enim tortor at auctor. Tempus egestas sed sed risus.
      </p>
    </crowd-tab>

    <crowd-tab header="Tab 2">
      <h2>Description</h2>
      <p>
      Sed risus ultricies tristique nulla aliquet enim tortor at auctor. Tempus egestas sed sed risus.
      </p>
    </crowd-tab>

    <crowd-tab header="Tab 3">
      <div style="width: 40%; display: inline-block">
        <img
          src="https://images.unsplash.com/photo-1472747459646-91fd6f13995f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
          style="max-width: 80%"
        >
        <crowd-input label="Input inside tab" name="inputInsideTab"></crowd-input>
        <input type="checkbox" name="checkbox" value="foo">Foo
        <input type="checkbox" name="checkbox" value="bar">Bar
        <crowd-button>Some button</crowd-button>
      </div>

      <div style="width: 40%; display: inline-block; vertical-align: top">
        Lorem ipsum dolor sit amet, lorem a wisi nibh, in pulvinar, consequat praesent vestibulum tellus ante felis auctor, vitae lobortis dictumst mauris. 
        Pellentesque nulla ipsum ante quisque quam augue. 
        Class lacus id euismod, blandit tempor mauris quisque tortor mauris, urna gravida nullam pede libero, ut suscipit orci faucibus lacus varius ornare, pellentesque ipsum. 
        At etiam suspendisse est elementum luctus netus, vel sem nulla sodales, potenti magna enim ipsum diam tortor rutrum, 
        quam donec massa elit ac, nam adipiscing sed at leo ipsum consectetuer. Ac turpis amet wisi, porttitor sint lacus ante, turpis accusantium, ac maecenas deleniti, 
        nisl leo sem integer ac dignissim. Lobortis etiam luctus lectus odio auctor. Justo vitae, felis integer id, bibendum accumsan turpis eu est mus eros, ante id eros. 
      </div>
    </crowd-tab>

  </crowd-tabs>

  <crowd-input label="Input outside tabs" name="inputOutsideTab"></crowd-input>

  <short-instructions>
    <p>Sed risus ultricies tristique nulla aliquet enim tortor at auctor. Tempus egestas sed sed risus.</p>
</short-instructions>

<full-instructions header="Classification Instructions">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
    <p> Tempus egestas sed sed risus.</p>
</full-instructions>

</crowd-form>
```

## Attributes<a name="tab-attributes"></a>

The following attributes are supported by this element\.

### header<a name="tab-attributes-header"></a>

The text appearing on the tab\. This is usually some short descriptive name indicative of the information contained below the tab\.

## Element Hierarchy<a name="tab-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-tabs](sms-ui-template-crowd-tabs.md)
+ **Child elements**: none 

## See Also<a name="tab-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)