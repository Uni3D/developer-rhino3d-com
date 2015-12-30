---
layout: code-sample-cpp
title: Maximize View
author: dale@mcneel.com
platforms: ['Windows']
apis: ['C/C++']
languages: ['C/C++']
keywords: ['rhino']
categories: ['Unsorted']
TODO: 0
origin: http://wiki.mcneel.com/developer/sdksamples/maximizeview
description: Demonstrates how to maximize a view.
order: 1
---

```cpp
CRhinoCommand::result CCommandTest::RunCommand(const CRhinoCommandContext& context)
{
  CRhinoGetString gs;
  gs.SetCommandPrompt( L"Name of viewport to maximize" );
  gs.GetString();
  if( gs.CommandResult() != CRhinoCommand::success )
    return gs.CommandResult();

  ON_wString view_name( gs.String() );
  view_name.TrimLeftAndRight();
  if( view_name.IsEmpty() )
    return CRhinoCommand::cancel;

  ON_SimpleArray<CRhinoView*> view_list;
  int view_count = context.m_doc.GetViewList( view_list );

  CRhinoView* active_view = NULL;
  int i;

  for( i = 0; i < view_count; i++ )
  {
    CRhinoView* view = view_list[i];
    if( view && view_name.CompareNoCase(view->Viewport().Name()) == 0 )
    {
      active_view = view;
      break;
    }
  }

  if( !active_view )
  {
    RhinoApp().Print( L"Unable to find viewport named %s\n", view_name );
    return CRhinoCommand::nothing;
  }

  ::RhinoApp().SetActiveView( active_view );
  CWnd* frame_wnd = active_view->GetParent();
  if( frame_wnd )
  {
    frame_wnd->ShowWindow( SW_SHOWMAXIMIZED );
    frame_wnd->BringWindowToTop();
  }
  return CRhinoCommand::success;
}
```