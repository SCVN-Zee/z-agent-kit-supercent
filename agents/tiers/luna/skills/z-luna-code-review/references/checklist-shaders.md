# Shaders and rendering

Use for shader code, render-pipeline changes, materials, runtime keyword toggles or depth sampling.

## Variant retention

Trace each runtime-enabled keyword to the intended pass/variant and the project's retained variants. V7 hints
699–707 (`SVC_Inspector_*`, `RuntimeAnalysisTab_PauseShaders`) exposes scene-used, user-included and
user-excluded variants plus stripping control; 913–916 explains Runtime Analysis removal. Hints 697
(`SVC_Element_ErrorMsgTemplate`) warns about selecting two keywords from the same shader_feature group.

A material keyword alone is not proof that the required variant survives every export setting.
`shader_feature_local` changes keyword scope; it is not evidence of retention and must not be offered as a
guaranteed stripping fix. Recommend exercising the path in a Develop build or explicitly retaining the
required variant, then verify the export. Do not permanently disable all stripping without considering build
size.

## Version-dependent rendering checks

| Surface | Required evidence before reporting a defect |
| --- | --- |
| Shader target, ShaderLab tags and Fallback | Installed compiler diagnostics and current documented subset. Do not enforce the old universal target ≤3.0, tag blacklist or ShadowCaster-only fallback rule without matching evidence. |
| HDRP, URP or Built-in pipeline | Installed package/version support and export diagnostics. A pipeline name alone does not establish compatibility of every shader/feature; do not auto-migrate rendering pipelines. |
| _CameraDepthTexture | Reproduce required texture creation, sampling, browser/WebGL support and render ordering. An identifier match is not proof of GL_INVALID_OPERATION; a fresnel approximation changes the effect. |
| Header/Tooltip punctuation | Reproduce a parser error in the installed exporter before restricting text to alphanumeric characters. Preserve harmless authoring labels. |
| Ambient/environment lighting | V7 hints 918 (`Basic_ProjectSettings_BakeReferenceAmbientProbe_Hint`) describes baking probe data for Built-in and warns that runtime skybox/lighting changes may look different. Inspect those changes when the symptom matches. |

Read material and shader state with connected read-only capabilities. Missing access leaves checks unverified.
Report actual export diagnostics with their observed codes; do not map magenta, a missing variant or any
shader failure to an assumed LP1001.
