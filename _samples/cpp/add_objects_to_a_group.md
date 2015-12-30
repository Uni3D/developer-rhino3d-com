---
layout: code-sample-cpp
title: Add Objects to a Group
author: dale@mcneel.com
platforms: ['Windows']
apis: ['C/C++']
languages: ['C/C++']
keywords: ['rhino']
categories: ['Unsorted']
TODO: 0
origin: http://wiki.mcneel.com/developer/sdksamples/addobjectstogroup
description: Demonstrates how to add selected objects to an object group.
order: 1
---

```cpp
CRhinoCommand::result CCommandTest::RunCommand(const CRhinoCommandContext& context)
{
  CRhinoGetObject go;
  go.SetCommandPrompt( L"Select objects to group" );
  go.EnableGroupSelect();
  go.GetObjects(1,0);
  if( go.CommandResult() != CRhinoCommand::success )
    return go.CommandResult();

  int i = 0, count = go.ObjectCount();
  ON_SimpleArray<const CRhinoObject*> members( count );

  for( i = 0; i < count; i++ )
    members.Append( go.Object(i).Object() );

  int index = context.m_doc.m_group_table.AddGroup( ON_Group(), members );
  context.m_doc.Redraw();
  return (index >= 0) ? CRhinoCommand::success : CRhinoCommand::failure;
}
```