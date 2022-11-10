## マスク適用
ShaderGraphで作ったシェーダーはそのままではマスクを適用できない。  
Stencil系のプロパティが必要。  
.shaderGraphファイルから.shaderコードを生成して以下の変更を加えるとマスクが適用できるようになる。    
```
...
    Properties
    {
        [NoScaleOffset]_MainTex("MainTex", 2D) = "white" {}
        [NoScaleOffset]_NoiseTex("NoiseTex", 2D) = "white" {}
        _DistortionSeedX("DistortionSeedX", Float) = 0
        _DistortionSeedY("DistortionSeedY", Float) = 0
        _DistortionStrength("DistortionStrength", Float) = 0.4
        [HideInInspector][NoScaleOffset]unity_Lightmaps("unity_Lightmaps", 2DArray) = "" {}
        [HideInInspector][NoScaleOffset]unity_LightmapsInd("unity_LightmapsInd", 2DArray) = "" {}
        [HideInInspector][NoScaleOffset]unity_ShadowMasks("unity_ShadowMasks", 2DArray) = "" {}

        // -------- 追加 --------
        _Stencil("Stencil ID", Float) = 0
        _StencilComp("StencilComp", Float) = 8
        _StencilOp("StencilOp", Float) = 0
        _StencilReadMask("StencilReadMask", Float) = 255
        _StencilWriteMask("StencilWriteMask", Float) = 255
        _ColorMask("ColorMask", Float) = 15
        // -------- 追加 --------
    }
    
...

    SubShader
    {
        Tags
        {
            "RenderPipeline"="UniversalPipeline"
            "RenderType"="Transparent"
            "UniversalMaterialType" = "Unlit"
            "Queue"="Transparent"
        }
        Pass
        {
            Name "Pass"
            Tags
            {
                // LightMode: <None>
            }

            // Render State
            Cull Off
        Blend SrcAlpha OneMinusSrcAlpha, One OneMinusSrcAlpha
        
        // -------- 変更 --------
        ZTest [unity_GUIZTestMode]
        // -------- 変更 --------
        
        ZWrite Off

        // -------- 追加 --------
        Stencil{
            Ref [_Stencil]
            Comp [_StencilComp]
            Pass [_StencilOp]
            ReadMask [_StencilReadMask]
            WriteMask [_StencilWriteMask]
        }
        // -------- 追加 --------
...

    SubShader
    {
        Tags
        {
            "RenderPipeline"="UniversalPipeline"
            "RenderType"="Transparent"
            "UniversalMaterialType" = "Unlit"
            "Queue"="Transparent"
        }
        Pass
        {
            Name "Pass"
            Tags
            {
                // LightMode: <None>
            }

            // Render State
            Cull Off
        Blend SrcAlpha OneMinusSrcAlpha, One OneMinusSrcAlpha
        
        // -------- 変更 --------
        ZTest [unity_GUIZTestMode]
        // -------- 変更 --------
        
        ZWrite Off

        // -------- 追加 --------
        Stencil{
            Ref [_Stencil]
            Comp [_StencilComp]
            Pass [_StencilOp]
            ReadMask [_StencilReadMask]
            WriteMask [_StencilWriteMask]
        }
        // -------- 追加 --------
...
```
