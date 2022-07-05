# Onshape Research Guide
<section class="section">
    <div class="container">
        <div>
            Onshape is the only Software-as-a-Service (SaaS) product development platform that combines CAD, built-in PDM, real-time collaboration tools, and business analytics. The unique architecture of the software provides a great potential for academic research and teaching in engineering, including but not limited to the following topics: 
        </div>
        <ul>
            <li>Research in individual design behaviour </li>
            <li>Research in synchronous collaboration with CAD </li>
            <li>Research in the characteristics of CAD models </li>
            <li>Teaching engineering science concepts with interactive models </li>
        </ul>
        <div>
            With its SaaS architecture, Onshape enables both unintrusive data collection and efficient data access, perfectly scalable for large-scale experiments and studies. This Onshape Research Guide aims to provide an introduction to some of the most popular sources of data in Onshape that may support academic research and teaching along with a few sample projects and analysis methods. If you would like to open-source your research method that relates to Onshape and want to be featured in this guide, please submit a pull request to <a href="https://github.com/PTC-Education/Onshape-Research-Guide">this GitHub repository</a> with details of the method. <br></br>
        </div>
        <div>
            This guide was initially built in collaboration with the <a href='https://readylab.mie.utoronto.ca'>Ready Lab</a> at the University of Toronto. For all publications that are benefited by this guide, please cite: <br></br>
        </div>
        <div>
            <b>NOTE:</b> this guide is aimed to serve and be used as an introduction to the technologies for the Onshape research community. We do not assure the technical accuracy of all details included in this guide, but please feel free to report any issues to <a href="https://github.com/PTC-Education/Onshape-Research-Guide">this GitHub repository</a>. <br></br>
        </div>
    </div>
    <div class="container">
        <h2>
            <a href="https://github.com/PTC-Education/Onshape-Research-Guide/analytics"><img src="resources/Data Process.svg" width="100px" alt=""/>Enterprise Analytics</a>
        </h2>
        <div>
            Every Onshape Enterprise account automatically comes with the <a href="https://cad.onshape.com/help/Content/EnterpriseHelp/Content/reports.htm?tocpath=Enterprise%7CAccessing%20Analytics%7C_____0">Analytics portal</a>. As users make edits and commit actions in an Onshape document, the history of these user behaviour is automatically logged in a chronological order. Researchers and teachers can build <a href='https://cad.onshape.com/help/Content/my_analytics.htm?tocpath=Enterprise%7CAccessing%20Analytics%7C_____1'>dashboards</a> to gain an overview of the enterprise. Through exporting the action history in the form of an <a href="https://cad.onshape.com/help/Content/audit_reports.htm?tocpath=Enterprise%7CAccessing%20Analytics%7C_____2">audit trail</a>, one can even study these logged actions by essentially rebuilding the design process for research analysis.
        </div>
        <div>
            <b>Onshape Enterprise Analytics enables the examniation of all actions in the design <i>process</i>.</b> <br></br>
        </div>
    </div>
    <div class="container">
        <h2>
            <a href="https://github.com/PTC-Education/Onshape-Research-Guide/api"><img src="resources/Database Application.svg" width="100px" alt=""/>REST API</a>
        </h2>
        <div>
            Onshape uses <a href="https://onshape-public.github.io/docs/apioverview/">REST APIs</a> to communicate with third party systems. With a library of API endpoints available on <a href="https://cad.onshape.com/glassworks/explorer/#/">Glassworks</a>, one can easily retrieve information from and make updates to an Onshape document. Further, establishing computational workflows with API calls allow scalable research analysis to a large amount of documents and data.</a>
        </div>
        <div>
            <b>The REST API provides efficient access to the design <i>product</i>.</b> <br></br>
        </div>
    </div>
    <div class="container">
        <h2>
            <a href="https://github.com/PTC-Education/Onshape-Research-Guide/ui_integration"><img src="resources/Systems Integration.svg" width="100px" alt=""/>External Integration</a>
        </h2>
        <div>
            With the <a href='https://onshape-public.github.io/docs/oauth/'>OAuth authorization framework</a>, one can integrate a locally launched web application to the native Onshape user interface. Being able to access and interact with information inside the Onshape document may enable a greater potential of using Onshape for teaching engineering science concepts to a class of students. 
        </div>
        <div>
            <b>External integration to the Onshape user interface allows more <i>interaction</i> with the design.</b> <br><br>
        </div>
    </div>
</section>
